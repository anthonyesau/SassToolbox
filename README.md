# SassToolbox
Collection of Sass tools

To use the entire set of tools, add this repository into your Sass directory and insert the following line at the beginning of the main Sass file:

```scss
import 'sass-tools/toolbox';
```

Or you can pick and choose the specific tools to include:

```scss
import 'sass-tools/swatchbook';
```

##1. Swatchbook

The color component is allows you to easily set multiple palettes and refer to colors within them.

Create some palettes like this. The palettes have individual names. Colors are named by the way they are used across multiple palettes.

```scss
$color-palettes: (
  light: (fg: #333, bg: #f8f8f8, accent: #ba9f61, fg-subtle: #808080, bg-subtle: #e5e5e5),
  dark: (fg: #f8f8f8, bg: #333, accent: #ba9f61, fg-subtle: #ccc, bg-subtle: #808080 ),
  accent: (fg: #f8f8f8, bg: #ba9f61, accent: #333, fg-subtle: #808080, bg-subtle: #ccc),
);
```

The first palette defined is set as the default, but you can change it to whatever you want:

```scss
@include set-palette-default(dark); // sets the dark palette as default
@include set-palette-default(3); // sets the 3rd palette (accent) as default
@include set-palette-default((fg: #f8f8f8, bg: #333, accent: #ba9f61, fg-subtle: #ccc, bg-subtle: #808080 )); //sets the map of colors provided as the default palette
```

Then access a color like this:
```scss
.foo {
  color: color(bg);
}
```

Which will render to css like this:
```css
.foo {
  color: #f8f8f8;
}
```

To define a property color value within multiple color palettes:
```scss
.accentline {
  @include color {
      border: 1px solid color(accent); //for each palette, this will always be the accent color
  }
}
```

It doesn't matter if the mixin is called on the outside of the selector. This renders like this:
```css
.primary-palette .accentline {
  border: 1px solid #ba9f61;
}
.primary-palette .accentline {
  border: 1px solid #ba9f61;
}
.dark-palette .accentline {
  border: 1px solid #ba9f61;
}
.accent-palette .accentline {
  border: 1px solid #333;
}
```
