---
layout: post
title:      "Index, Show, and Create without a page refresh"
date:       2018-10-14 20:55:11 -0400
permalink:  index_show_and_create_without_a_page_refresh
---

My Rails project was a concert management app where concerts have many attendees and attendees have many concerts. I added JavaScript to create dynamic features including indexing concerts, showing a concert, and creating a concert, that can all be done without refreshing the browser. 

**Index**

Handlebars made it very easy to iterate over a list of concerts and create a new HTML table:

```
<table>
  <tr>
    <th>Title</th>
    <th>Date</th>
  </tr>
  {#each this}
    <tr>
      <td><a href="#" class="concert-link" id={ id }>{ title }</a></td>
      <td>{ displayDate }</td>
    </tr>
  {/each}
</table>

<br>

<a href="#" id="create-concert-link">Create a Concert</a>
```

**Show**

The above index created links that were hijacked to send JSON GET requests for specific concerts:
```
function addConcertLinkListeners() {
  $("a.concert-link").on("click", function(e) {
    e.preventDefault();
    const id = this.id
    $.getJSON("/concerts/" + id, function(data) {
      showConcert(data);
    });
  });
}
```

**Create**

I originally spent a lot of time trying to figure out how to use Handlebars to render a new concert form when the "Create a Concert" link was clicked. This was not able to work because Rails requires an authenticity token on all form submissions. The solution ended up being very simple by adding one line of code to my ConcertsController: `render "concerts/new", layout: false`

This provided the exact HTML needed to append to the DOM - and it kept my app DRY by reusing the new form I had already written. 

