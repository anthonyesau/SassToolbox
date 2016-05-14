# Responder

Responder is a tool for managing and easily accessing media queries.

To start, you will need to define your queries in the `$media-queries` variable. The proper format is a map consisting of keys (the names of your queries) that are assigned a list of rules. The first two rules are the min and max width. Or they may be a `<` or `>` operator followed by a single width. You may then add any additional rules in the syntactically correct order required by media queries.

```scss
$media-queries: (
  sm: ("<", 480px),
  md: (481px, 728px, "only screen"),
  lg: (729px, 1280px),
  xlg: (">", 1280px)
);
```

## Accessing Queries

To access a media query, invoke the command `@include media(/*key*/) { /*content*/ }`. The `media` mixin takes several possible arguments:
* The name of a query (that you prefiously defined in the `$media-queries` variable)
* An operator (`<`, `>`, `<=`, or `>=`) followed by a query name
* A list of two widths, the start and end values for a width range
* A list consisting of an operator (`<`, `>`, `<=`, or `>=`) followed by a single width


```scss
@include media(lg) {
  /*content*/
}
@include media("<=md") {
  /*content*/
}
@include media(200px, 540px) {
  /*content*/
}
@include media(">", 540px) {
  /*content*/
}

//These become

@media (min-width: 729px) and (max-width: 1280px) {
  /*content*/
}
@media (max-width: 728px) {
  /*content*/
}
@media (min-width: 200px) and (max-width: 540px) {
  /*content*/
}
@media (min-width: 540px) {
  /*content*/
}
```


## Responsive Values

Another useful feature is management for standard values that vary across media queries. To create your sets, define the `$responsive-values` variable as a map with keys (the name of each set) that contain the list of changing values. The order should correspond with the order of the queries within the `$media-queries` variable. So the first value in a set within `$responsive-values` will correspond with the first query in `$media-queries`. The second value in a set within `$responsive-values` will correspond with the second query in `$media-queries` and so on.

```scss
$responsive-values: (
  foo: ("first" "second" "third" "fourth"),
  bar: ("apples" "bananas" "oranges" "pineapples" "cherries")
) !default;
```

To access these values use the `responsive-value()` function and pass the name of a set as the argument.

```scss
@include media(sm) {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
}
@include media(md) {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
}
@include media(lg) {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
}

//becomes

@media (max-width: 480px) {
  /*apples*/
  /*first*/
}
@media (min-width: 481px) and (max-width: 728px) {
  /*bananas*/
  /*second*/
}
@media (min-width: 729px) and (max-width: 1280px) {
  /*oranges*/
  /*third*/
}
```

A responsive value can be accessed at any time. However, when it is called outside of the `media mixin`  it will access the value corresponding to the query as defined in the `$media-query-index` variable.

```scss
$media-query-index: 3; //access the 3rd query
/*#{responsive-value(bar)}*/
/*#{responsive-value(foo)}*/

//become

/*oranges*/
/*third*/
```

Every time the `media mixin` is called, it resets `$media-query-index` to a default value. Set this default with the `$media-query-index-default` variable.

```scss
$media-query-index-default: 2;
```

## Multiple Queries

To easily iterate through all the queries, invoke the `multi-media()` function.

```scss
@include media(sm) {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
}
@include media(md) {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
}
@include media(lg) {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
}

// The above code can be condensed down to

@include multi-media {
  /*#{responsive-value(bar)}*/
  /*#{responsive-value(foo)}*/
};
```
