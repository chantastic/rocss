ROCSS
=====

A sensible approach to resource heavy stylesheets.

## TL;DR

Contemporary applications represent resource data in two ways:

1. As a detailed description
1. As an item in a list

Most applications don't have a pattern for writing CSS classes in a way that
reflects this UI pattern. I recommend this:

```css
.resource {}
.resource-item {}
```

In Rails, that might look like so:

```css
.resource {}
._resource {}
```

## The Basic Idea

There are two ways that we see see a resource represented in RESTful systems.

* Single Resource          — `"resoure": { "id": 1 }`
* Resource Collection Item — `"resources": [{ "id": 1 }]`

When put in a UI, these rarely look similar. The first is used to _show_ the
full detail of a resource, while the second represents an actionable subset of
that resources data.

The canonical example of this is an email client:

```
+----------------------------------------------------------------------------------+
|                                   * Email *                                      |
+----------------------------------------------------------------------------------+
|                              |                                                   |
|  Someone:  Totally Impo...  <   From: Someone                                    |
|                              |  To:   Me                                         |
|------------------------------|                                                   |
|                              |  Subject: Totally important                       |
|  Mistress: We really ne...   |                                                   |
|                              |                                                   |
|------------------------------|  Hello Me,                                        |
|                              |                                                   |
|  Buddy: Bro! check it out!   |  This is totally an important message. Take ...   |
|                              |                                                   |
+----------------------------------------------------------------------------------+
```

On the left there you have _item_ representations of resources with a _full_
resource on the right.

The class names for each presentation should be consistent from resource to
resource. I use this:

```css
.resource {}       /* .email {}      */
.resource-item {}  /* .email-item {} */
```

### Don't like -item?

Some people don't the `-item` suffix. Fortunately, that part
really isn't important. Name it however you like.

A pattern that I like in Rails is to use an `_` prefixed. It's less
communicative but maps well with Rails conventions.

```css
.person  {}
._person {}
```

_Hat-tip to @danott_

## A Single Resource (show)

Name a single resource after its resource name.

JSON
```json
{
  "person": {}
}
```

HTML
```html
<div class="person">
  ...
</div>
```

## Naming A Resource Collection Item (list)

Name a collection item after its singularized resource name with a `-item` suffix.

JSON
```json
{
  "people": [
    { "id": 1 },
    { "id": 2 }
  ]
}
```

HTML
```html
<ul>
  <li class="person-item">...</li>
  <li class="person-item">...</li>
</ul>
```

## Example: Email Client (Single and Collection)


JSON
```json
{
  "email": [
    { "id": 1, "from": "Someone", ... },
    { "id": 2, "from": "Mistress", ... },
    { "id": 3, "from": "Buddy", ... }
  ]
}
```

HTML
```html
<div class="email" id="1">
  <h4>From: {email.from}</h4>
  <h4>Subject: {email.subject}</h4>

  <p>From: {email.body}</p>
</div>

<ul>
  <li class="email-item" id="2">
    {email.from}
  </li>
  <li class="email-item" id="3">
    {email.from}
  </li>
</ul>
```

## Nested Resources

Nested resources become very natural to handle with this pattern.

JSON
```json
{
  "person": {
    "favorite_things": [
      { "name": "raindrops on roses" },
      { "name": "whiskers on kittens" }
    ]
  }
}
```

HTML
```html
<div class="person">
  <ul>
    <li class="favorite-thing">...</li>
    <li class="favorite-thing">...</li>
  </ul>
</div>
```

## Nested Resources with Relationship-Specific Style

Create overrides for specific relationships using OOCSS-style sub-classing..

JSON
```json
{
  "person": {
    "favorite_things": [
      { "name": "raindrops on roses" },
      { "name": "whiskers on kittens" }
    ]
  }
}
```

HTML
```html
<div class="person">
  <ul>
    <li class="favorite-thing person-favorite-thing">...</li>
    <li class="favorite-thing person-favorite-thing">...</li>
  </ul>
</div>
```

CSS
```css
.favorite-thing {
  // default styles
}

.person-favorite-thing {
  // .person specific .favorite-thing extensions
}
```

*DO NOT cascade from a parent*

```css
/* BAD */

.favorite-thing {}

.person .favorite-thing {}
```

#### Why No Cascade?

The only thing that `person` and `favorite-thing` share is layout proximity. Change
the layout, break the stylesheet. Don't be confused; proximity is coincidental.

If `favorite-thing` needs different rules, subclass it.
