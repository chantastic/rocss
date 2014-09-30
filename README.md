ROCSS
=====

A sensible approach to resource-driven stylesheets.

## TL;DR

Resource Oriented CSS is better naming conventions for resource heavy
stylesheets.

## How To Read This Document

The patterns are prescriptive. The execution is up to you. I'm using BEM and
OOCSS to describe my markup objects. You're free to use whatever you seed fit.

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
