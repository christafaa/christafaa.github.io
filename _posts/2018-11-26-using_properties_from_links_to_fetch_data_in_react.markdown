---
layout: post
title:      "Using properties from links to fetch data in React"
date:       2018-11-27 00:00:55 +0000
permalink:  using_properties_from_links_to_fetch_data_in_react
---


My React Redux project is a venue seat manager that allows a user (for example, a box-office attendant) to select an event, view the event's available seats, and assign seats to an attendee. 

Each concert available to view is represented as a NavLink that directs to `/concerts/:id` 
When a user clicks on an event to see the event's details, I need access to the `id` parameter from the NavLink to fetch the correct data from my Rails API. The react-router package makes this very easy for us by giving `<Route>` access  to a `match` object that contains the following properties: 

* `params` - (object) Key/value pairs parsed from the URL corresponding to the dynamic segments of the path
* `isExact` - (boolean) true if the entire URL was matched (no trailing characters)
* `path` - (string) The path pattern used to match. Useful for building nested `<Route>`s
 * `url` - (string) The matched portion of the URL. Useful for building nested `<Link>`s

When a user clicks to view the list of attendees for a particular event, the `<Route>` is written like this:
          `<Route exact path="/concerts/:id/attendees" render={({match}) => <AttendeesContainer concertId={match.params.id}/>}/>`
					
Notice that we are using `match`'s `params` properity to access `id`!

Once the `AttendeesContainer` mounts, we fetch the attendees of the correct event:
					
```
class AttendeesContainer extends Component {

  componentDidMount(id) {
    this.props.fetchAttendees(this.props.concertId)
  }
	...other code...
}
```

Source: [react-router docs](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/match.md)


