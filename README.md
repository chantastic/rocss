ROCSS
=====

A sensible approach to resource-driven stylesheets.

## TL;DR

Resource Oriented CSS is better naming conventions for resource heavy
stylesheets.

## How To Read This Document

The examples in this readme use BEM and OOCSS for illustration. But this
approach is at home in any CSS naming system.

## The Basic Idea

The idea is simple: have unique names for common representations of a resource.

The APIs we interact with predominantly return JSON. In JSON, you have ways a
resource might be represented:

* Single Resource          — "person": {}
* Resource Collection      — "people": []
* Resource Collection Item — "people": [{ "id": 1 }]

The first two are obvious. The third is less obvious. Your instinct might be to
represent the third like this:

```html
<ul class="people">
  <li class="person"></li>
  <li class="person"></li>
</ul>
```

This seems harmless. In fact, it likely maps to verbiage in your tamplate perfectly.
But what happens when when your layout grows to include a dialog which shows the
single resource fully? You've used `.person` to describe the iterated resources.
Now you must decide what to name your representation of the single resource.

This problem continues as you go to style a 'show' page. You can borrow
styles from `.person` but you end up redefining most of the class and eventually
forking it off into something unsightly, like `.person-full`, `.person-big`, or
worse `.person-show`.

You can avoid this completely by considering the regular resource-types up front:

```css
.person {}      /* a single resource */
.person-list {} /* a resource collection */
.person-item {} /* a resource collection item */
```

## A Word on Alternatations

There are a number of possibilities naming. If the ones I've suggested don't
bother you, use them. They are the most universally useful.

I've also tried these configurations:

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

These are both fine. But there were resources, layouts, and circumstances that
make these hard to talk and reason about. I no longer reach for these iterations.

Whatever you choose, be consistent.

## Naming A Single Resource (#show)

Name a single resource after its Class.

```json
{
  "person": {}
}
```

```html
<div class="person">
  ...
</div>
```

## Naming A Resource Collection (#list)

Name a collection after its singular Class with a `-list` suffix.


```json
{
  "people": [
    {},
    {}
  ]
}
```

```html
<ul class="person-list">
  ...
</ul>
```

#### Alternative

You may prefer `people` to `person-list`. I don't.


## Naming A Resource Collection Item (#list(each))


Name a collection item after its singular Class with a `-item` suffix.

You may be tempted to re-purpose `person`. Don't.


```json
{
  "people": [
    {},
    {}
  ]
}
```

```html
<li class="person-item">...</li>
<li class="person-item">...</li>
```


## Sample of Single and Collection of Resources

Consider a contact-list. You have a layout featuring a single person, from a
list of people.

```json
{
  "people": [
    { "id": 1 },
    { "id": 2 },
    { "id": 3 }
  ]
}
```

```html
<div class="person" id="1">
  ...
</div>

<ul class="person-list">
  <li class="person-item" id="2">...</li>
  <li class="person-item" id="3">...</li>
</ul>
```

## Handling Nested Resources

If `-list` and `-item` conventions are followed, nested resources are naturally
handled.


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
    <li class="favorite-thing">...</li>
    <li class="favorite-thing">...</li>
  </ul>
</div>
```


## Handling Nested Resources with Relationship Specific Styles

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
