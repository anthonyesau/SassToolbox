# Fluid

Avoid jarring breakpoints for text or margin sizes by using fluid calculations.

Only works for numerical values that can be interpolated from a range.

## Usage
Define the minimum and maximum ranges for each desired fluid value in a list variable called `$fluid-values`. It should be formated as a list of nested lists. Top level keys are each associated with a minimum and maximum value. The `width` key is required as it sets the screen width range across which all other values will be interpolated.

```scss
$fluid-values: (
  width: (640px 1280px),
  baserem: (18px 21px),
  text-2: (.382rem .382rem),
  text-1: (.618rem .618rem),
  text0: (1rem 1rem),
  text1b: (1.414rem 1.414rem),
  text1: (1.618rem 1.618rem),
  text2: (2.618rem 2.618rem),
  grid-gutter: (2rem 4rem)
);
```

Wrap styles in the `fluid` mixin and assign properties with values of `fluid-values(key)` or `fv(key)`.

```scss
@include fluid{
  :root {
    font-size: fv(baserem);
  }
};
```

To create a one-off fluid value without setting a key in `$fluid-values`, use `manual-fluid-value($min_val, $max_val, $min_width, $max_width)` or `mfv($min_val, $max_val, $min_width, $max_width)`. If you do not define them, `$min_width` and `$max_width` will default to the width values set in `$fluid-values`.

```scss
@include fluid{
  :root {
    font-size: mfv(18px, 21px, 640px, 1280px);
  }
};
```

## Credit

Based on code by Mike Riethmuller. See his [demo](http://codepen.io/MadeByMike/pen/YPJJYv?editors=110) or read more about the method at a post on his website, [Precise control over responsive typography](https://madebymike.com.au/writing/precise-control-responsive-typography/).
