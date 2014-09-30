ROCSS
=====

A sensible approach to resource heavy stylesheets.

## What's Important?

There's only one take-away: choose a convention for three resource types up
front and stick with it.

## The Basic Idea

We work with JSON APIs. JSON is predictable. You have three ways a might be
represented:

* Single Resource          — "person": {}
* Resource Collection      — "people": []
* Resource Collection Item — "people": [{ "id": 1 }]

The first two are obvious. The third is less obvious. Your instinct is likely to
represent the third resource type like this:

```html
<ul class="people">
  <li class="person"></li>
  <li class="person"></li>
</ul>
```

This seems harmless. In fact, verbiage probably maps to your template perfectly.
But what happens when when your layout grows to include a dialog which shows a
full representation of a single resource? You've used `.person` to describe the
iterated resources.  Now you're forced to decide what to name your single resource.

This problem continues as you go to style a 'show' page. You can borrow
styles from `.person` but you'll end up redefining most of the class and eventually
forking it into something unsightly, like `.person-full`, `.person-big`, or
worse `.person-show`.

You can avoid this completely by considering common resource-types up front:

```css
.person {}      /* a single resource */
.person-list {} /* a resource collection */
.person-item {} /* a resource collection item */
```

## Don't Like -list and -item?

You're not alone. But don't fixate the execution isn't the important part.

I've tried these:

```css
/* pluralized */
.person {}
.people {}
.people-each {}
```

```css
/* verbose */
.person {}
.person-list {}
.person-list-item {}
```

These are both fine. The trouble is that they felt unnatural in some layouts, or
when working with oddly named resources. Opportunities arise when I might need
an `-item` representation without the context of a `-list`.

Choose and be consistent. "How" doesn't matter.

## A Single Resource (show)

Name a single resource after its Class.

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

## Naming A Resource Collection (list)

Name a collection after its singular Class with a `-list` suffix.

JSON
```json
{
  "people": [
    {},
    {}
  ]
}
```

HTML
```html
<ul class="person-list">
  ...
</ul>
```

You may prefer `people` to `person-list`. I don't.

## Naming A Resource Collection Item (each)

Name a collection item after its singular Class with a `-item` suffix.

You may be tempted to re-purpose `person`. Don't.

JSON
```json
{
  "people": [
    {},
    {}
  ]
}
```

HTML
```html
<li class="person-item">...</li>
<li class="person-item">...</li>
```

## Example: Contact List (Single and Collection)

Consider a contact-list. You have a layout showing a single person, from a
list of people.

JSON
```json
{
  "people": [
    { "id": 1 },
    { "id": 2 },
    { "id": 3 }
  ]
}
```

HTML
```html
<div class="person" id="1">
  ...
</div>

<ul class="person-list">
  <li class="person-item" id="2">...</li>
  <li class="person-item" id="3">...</li>
</ul>
```

## Nested Resources

If `-list` and `-item` conventions are followed, nested resources are handled
out of the box.

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
  <ul class="favorite-thing-list">
    <li class="favorite-thing">...</li>
    <li class="favorite-thing">...</li>
  </ul>
</div>
```

## Nested Resources with Relationship-Specific Style

Create overrides for specific relationships using composition.

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

```html
<div class="person">
  <ul class="favorite-thing-list">
    <li class="favorite-thing person-favorite-thing">...</li>
    <li class="favorite-thing person-favorite-thing">...</li>
  </ul>
</div>
```

```css
.favorite-thing {
  // default styles
}

.person-favorite-thing {
  // .person specific .favorite-thing overrides
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
the layout, break the stylesheet. Don't be confused. This proximity is a coincidence.

If `favorite-thing` needs different rules, subclass it.
