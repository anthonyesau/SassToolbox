# Fluid

Avoid jarring breakpoints for text or margin sizes by using fluid calculations.

Only works for numerical values that can be interpolated between a range. Units must be consistent, including the values set for the viewport width range.

## Usage
Define the minimum and maximum ranges for each desired fluid value in a list variable called `$fluid-values`. It should be formatted as a list of nested lists. Top level keys are each associated with a minimum and maximum value. The `vw` key is required as it sets the viewport width range across which all other values will be interpolated.

```scss
$fluid-values: (
  vw: (400px 1920px),
  baserem: (16px 24px),
  text-2: (.6rem .382rem),
  text-1: (.8rem .618rem),
  text0: (1rem 1rem),
  text1: (1.5rem 1.618rem),
  text2: (2rem 2.618rem),
  grid-gutter: (2rem 4rem)
);
```

Wrap styles in the `fluid` mixin and assign properties with values of `fluid-values(key)` or `fv(key)`.

```scss
@include fluid() {
  :root {
    font-size: fv(baserem);
  }
};
```
NOTE: The above code will create a fluid "base rem" for any measurement in rems.

To create a one-off fluid value without setting a key in `$fluid-values`, use `manual-fluid-value($min_val, $max_val, $min_width, $max_width)` or `mfv($min_val, $max_val, $min_width, $max_width)`. If you do not define them, `$min_width` and `$max_width` will default to the width values set in `$fluid-values`.

```scss
@include fluid() {
  :root {
    font-size: mfv(16px, 24px, 400px, 1920px);
  }
};
```

The assumed use-case for this tool is to create a fluid base rem which scales every object on the page according to viewport width. To extend this interpolation further and fluidly transition between two different values measured in rems, invoke the `fluid-rem($key)` function (`fr()` for short). This is an exception to the unit consistency rule as the viewport widths will be automatically converted to their rem equivalents.

```scss
@include fluid() {
  margin: fr(grid-gutter);
}
```

## Credit

Based on code by Mike Riethmuller. See his [demo](http://codepen.io/MadeByMike/pen/YPJJYv?editors=110) or read more about the method at a post on his website, [Precise control over responsive typography](https://madebymike.com.au/writing/precise-control-responsive-typography/).

## To Do

* [ ] Refine fluid calc formula (esp. to avoid bad math of strip-unit)
* [ ] Option to disable minimum or maximum size (remove stops at top and bottom ends)
