# Sass Tools

To use the entire set of tools, add this repository into your Sass directory and insert the following line at the beginning of the main Sass file:

```scss
@import 'sass-tools/toolbox';
```

Or you can pick and choose the specific tools to include:

```scss
@import 'sass-tools/swatchbook';
```

##1. Swatchbook

The color component is allows you to easily set multiple palettes and refer to colors within them.

### Define Palettes

Create some palettes like this. Palettes have individual names. Colors may also be individually named, however a useful strategy is to utilize uniform labels for the ways colors are used across multiple palettes. So instead of calling a specific color "beige" you may call it "bg" in the light color palette and "fg" in the dark palette.

```scss
$color-palettes: (
  light: (fg: #333, bg: #f8f8f8, accent: #ba9f61, fg-subtle: #808080, bg-subtle: #e5e5e5),
  dark: (fg: #f8f8f8, bg: #333, accent: #ba9f61, fg-subtle: #ccc, bg-subtle: #808080 ),
  accent: (fg: #f8f8f8, bg: #ba9f61, accent: #333, fg-subtle: #808080, bg-subtle: #ccc),
);
```

Define the `$color-palettes` variable prior to importing the Sass tools. Otherwise you will need to run several commands to implement your palettes:

```scss
@include set-palette-default(); //Sets the default palette
@include set-palette-temp(); //Sets the operational palette variable
```

The first palette defined is set as the default, but you can change it to whatever you want at any point within your Sass file:

```scss
@include set-palette-default(dark); // sets the dark palette as default
@include set-palette-default(3); // sets the 3rd palette (accent) as default
@include set-palette-default((fg: #f8f8f8, bg: #333, accent: #ba9f61, fg-subtle: #ccc, bg-subtle: #808080)); //sets the map of colors provided as the default palette
```

### Accessing colors

Then access a color from the default palette using the `color()` function with the color name as a parameter:
```scss
.foo {
  color: color(bg);
}

// becomes

.foo {
  color: #f8f8f8;
}
```

### Accessing Palettes

Palettes can be used as class selectors. The `@include palette { //content }` mixin will iterate through all defined palettes creating a class selector containing all contents for each. It doesn't matter if the mixin is called within or without the selector.

```scss
.accentline {
  @include palette {
      border: 1px solid color(accent); //this will always be the color defined as 'accent' for each individual palette
  }
}

// or

@include palette {
  .accentline {
    border: 1px solid color(accent); //this will always be the color defined as 'accent' for each individual palette
  }
}

//becomes

.light-palette .accentline {
  border: 1px solid #ba9f61;
}
.dark-palette .accentline {
  border: 1px solid #ba9f61;
}
.accent-palette .accentline {
  border: 1px solid #333;
}
```
**NOTE:** *Pass a parameter containing 'placeholder' or '%' (`@include palette (placeholder){ //content }`) to create placeholder selectors rather than class selectors.*


To simply access the colors within one specific palette, supply the palette name as a parameter. This *will not* create a class selector for the single palette so you must include selectors within the content of the mixin.

```scss
@include palette (light) { // Sets palette
  p {
    background: color(bg);
    color: color(fg);
  }
} // End of palette usage

// becomes

p {
  background: #f8f8f8;
  color: #333;
}
```

An alternative to accomplish the same:

```scss
@include set-palette-temp(light); // Sets palette
p {
  background: color(bg);
  color: color(fg);
}
@include set-palette-temp; // Resets palette to default

// becomes

p {
  background: #f8f8f8;
  color: #333;
}
```
