---
layout: post
title:      "Using partials and helpers to create a lean Rails app"
date:       2018-08-29 22:34:52 -0400
permalink:  using_partials_and_helpers_to_create_a_lean_rails_app
---

**My Project:**

My [Rails project](https://github.com/christafaa/rails-concert-management) is a concert and donor management app and its intended user is a staff member at a non-profit presenting arts organization. A user can create a concert and a concert has many attendees through tickets. An attendee also belongs to a user, who acts as the attendee's solicitor and is responsible for cultivating the attendee, or "prospect", at concerts.

**Using a partial to refactor my views:**

I had code that looked very similar in three separate views in my app:
1. The attendees index page listed all attendees
2. A concert show page listed all attendees who had a ticket to that concert
3. A user show page listed all attendees who were prospects of that user

All three views have one thing in common: they display some kind of list of attendees.

In each view, I had a table and sort form: 

```
<table>
  <tr>
    <th>Name</th>
    <th>Profession</th>
    <th>Wealth Rating</th>
    <th>Total Season Tickets</th>
    <th>Solicitor</th>
  </tr>
  <% @attendees.each do |attendee| %>
    <tr>
      <td><%= link_to attendee.full_name, attendee_path(attendee) %></td>
      <td><%= attendee.profession %></td>
      <td><%= attendee.wealth_rating %></td>
      <td><%= attendee.tickets.count %></td>
      <td><%= link_to attendee.user.username, user_path(attendee.user) if attendee.user %></td>
      <td><%= link_to "Edit", edit_attendee_path(attendee) %></td>
    </tr>
  <% end %>
</table><br>

<%= "Sorted by: #{@sort_status}" %><br><br>

<%= form_tag(@path, method: "get") do %>
  <%= select_tag "sort", options_for_select(["Alphabetical", "Best Wealth Rating", "Most Season Tickets"]) %>
  <%= submit_tag "Sort" %>
<% end %>
```

The first step was to move this code to a partial so I could keep my views DRY. I did a small refactor with the sort form and moved the unique path of each view, `concert_path(@concert)`, for example, into the controller and saved it into an instance variable,`@path`. Since each view mentioned above used a different path, moving that logic to the controller allowed me to keep the partial flexible.

**Using a helper to refactor my controllers:**

The real challenge was removing the repetitive code in the controller actions which each used lengthy case statements to create the @attendees instance variable based on how a user wanted to sort the list of attendees. 

For example, the attendees#index action included the following conditional to check for a sort selection and then set the @attendees and @sort_status instance variables:

```
 if params[:sort]
	 case params[:sort]
	 when "Alphabetical"
		 @attendees = Attendee.alpha.uniq
		 @sort_status = "Alphabetical"
	 when "Best Wealth Rating"
		 @attendees = Attendee.best_wealth_rating.uniq
		 @sort_status = "Best Wealth Rating"
	 when "Most Season Tickets"
		 @attendees = Attendee.most_tickets.uniq
		 @sort_status = "Most Season Tickets"
	 end
 else
	 @attendees = Attendee.alpha.uniq
	 @sort_status = "Alphabetical"
 end
```

To remove this repetitive logic from my three controller actions (attendees#index, concerts#show, users#show), I moved it into an AttendeesHelper method:

```
def attendees_and_sort_status(collection, sort_method)
    if sort_method
      case sort_method
      when "Alphabetical"
        attendees = collection.alpha.uniq
        sort_status = "Alphabetical"
      when "Best Wealth Rating"
        attendees = collection.best_wealth_rating.uniq
        sort_status = "Best Wealth Rating"
      when "Most Season Tickets"
        attendees = collection.most_tickets.uniq
        sort_status = "Most Season Tickets"
      end
    else
      attendees = collection.alpha.uniq
      sort_status = "Alphabetical"
    end
    [attendees, sort_status]
  end
end
```

Now my three controller actions are skinny and they each set the @attendees and @sort_status instance variables in one line of code: `@attendees, @sort_status = helpers.attendees_and_sort_status(collection, params[:sort])`

The `collection` argument for attendees#index is simply `Attendee.all` but it would be different for concerts#show and users#show depending on *which concert* or *which user* we were looking at. I solved this problem by creating an additional Attendee class method: 

```
  def self.collection_of(association)
    where(id: association.attendees.map(&:id))
  end
```

This method takes an associated concert or user object, maps over the attendees collection of that concert or user to get each attendee's id, and then returns an Active Record Relation of attendees whose ids were included in the collection. Now I can call any sorting class method on this narrowed-down Active Record Relation. My final concerts#show looked like this:

```
  def show
    @concert = Concert.find(params[:id])
    @path = concert_path(@concert)
    collection = Attendee.collection_of(@concert)
    @attendees, @sort_status = helpers.attendees_and_sort_status(collection, params[:sort])
  end
```

Rails partials and helpers are great tools to help refactor code to keep your controllers skinny and your views DRY.





