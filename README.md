# Notes

CSS uses _late binding_: the content and its styles are not pulled together until after the authoring of both is complete.

The _root node_ is the ancestor of all elements in the document. It is `<html>`. `:root` is its special pseudo-class selector.

Define `line-height` on the body element so that it is inherited by rest of the document.

The original intention of vendor prefixes was to allow developers to experiment with technology before they used it in production. But developers started using it in production immediately. 

Use [Autoprefixer CSS online](https://autoprefixer.github.io/).

Shorthand properties:
```css
.item {
    /* Apply to all four sides */
    margin: 1em;
    margin: -3px;

    /* vertical | horizontal */
    margin: 5% auto;

    /* top | horizontal | bottom */
    margin: 1em auto 2em;

    /* top | right | bottom | left */
    margin: 2px 1em 0 auto;
}
```

### Feature queries

Set fallbacks for CSS features that might not yet be supported by all browsers. Use the `@supports` rule:

```css
@supports (display: grid) {
    ...
}

@supports (declaration) or (declaration) {
    ...
}

@supports (declaration) and (declaration) {
    ...
}
```
In the previous example, if the browser supports the grid layout, it applies the styles in the parentheses.

### Document flow
Don't set height on a container unless in special circumstances. Normal document flow is designed to work with a constrained width and unlimited height. The height of a container is determined by its contents, not the container.

When you set an element's height, you are in danger of _overflowing_ the container.

If you expect overflow, set it to `auto` so that the browser manages when to add scrollbars automatically.

Adjacent top or bottom margins combine (_collapse_) to form a single margin. Flex items do not collapse.

Always set up the a `flow` class using the lobotomized owl to target any element that immediately follows any other element:

```css
.flow * + * {
    margin-top: 1.5rem;
}
```

## Selectors

parent > child: `>` is a descendant selector that targets an element that is child of `parent`.

## Relative values

- A CSS pixel =/= to a monitor pixel
- **Computed value**: Absolute value that the browser computes for values declared using relative units
- Whe an element has a value defined using a length (px, rem, em, etc), its computed value is inherited by its child elements.

When to use rems:
- font sizing. Always use relative units when setting a font size because when users alter the screen zoom with + or -, pixels do not resize correctly.

When to use ems:
- padding
- margins
- element sizing
- border-radius

pixels:
- borders

percentages:
- container widths (as necessary)

## Box model

The following sets the box sizing on the entire document, while allowing you to set box sizing differently on specific components, if necessary:

```css
:root {
    box-sizing: border-box;
}

*,
*::before,
*::after {
    box-sizing: inherit;
}

.special-component {
    box-sizing: content-box;
}
```

## Flex

Flexbox allows you to define 1-D layouts. It works from the content out. Use it for rows or columns of similar elements. You don't need to set the size, because that is determined by the content.

Applying `display: flex` to an element turns it into a _flex container_, and all of its children become _flex items_. By default, flex items are the same height.

Flex items align side by side, left to right, in a row. Use `flex-direction` to change this.

A flex container takes up 100% of its available width, while the height is determined natually by its contents. The `line-height` of the text inside each flex item is what determines the height of each item.

Flex items are placed horizontally along the _main axis_, and vertically along the _cross-axis_.

Margins between flex items do not collapse like in the regular document flow.

Styling with flexbox involves the following:
- Identify the flex container and apply `flex: display;`
- Set `flex-direction`, if necessary.
- Declare margins or `flex` values for the flex items to control their sizes.

### Flex container properties

Apply the following properties to the flex container:

```css
.flex-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    /* flex-flow is shorthand for flex-direction and flex-wrap */
    flex-flow: column wrap;
    /* justify-content uses the main axis. Think about setting text. */
    justify-content: space-between;
    /* align-* props use the cross-axis */
    align-items: flex-start;
    align-content: stretch;
    gap: 1rem;
    row-gap: 1rem;
    column-gap: 1rem;
}
```

### Flex item properties

Apply the following properties to the flex container:

```css
.flex-item {
    order: 3;
    flex-grow: 1;
    flex-shrink: 2;
    flex-basis: auto;
    flex: 1;
    /* uses cross-axis */
    align-self: flex-start;
}

```
### flex: property
Use the `flex` property to to allow flex items to grow to fill its container size, and to size flex items relative to each other:
```css
.column-one {
    flex: 1;
}

.column-two {
    flex: 2;
}
```

In the previous example, `column-one` is 1/3 the size of the screen width, and `column-two` is 2/3 its size.

`flex` property is short for the following:
- `flex-grow`: Set to `0` or `1`:
  - `0`: The flex item will not grow past its `flex-basis`
  - `1` or non-zero: The flex item grows until all of the remaining space is used up. The item takes up more space relative to the `flex-grow` value set on other items. For example, setting to `2` makes it consume twice the space that a sibling element set to `1` consumes.
- `flex-shrink`: Default: 1. Determines if the element shrinks to prevent overflow. Set to `0` to prevent the item from shrinking.
- `flex-basis`: Default: 0%. Sets the starting point for the size of the element before the remaining space is distributed. The initial size is `auto`, which means it checks for a declared width and uses that, or it uses the element's contents for sizing.

#### Holy Grail layout:

The first and third columns have a fixed width of 200px, and the center column grows to fill all the remaining space:

 ```css
.column-one {
    flex: 0 0 200px;
}

.column-two {
    flex: 1;
}

.column-three {
    flex: 0 0 200px;
}
```

### Flex direction

Swaps the direction of the main-axis and the cross-axis. 

For `flex-direction` is a row, the `flex` property applies to the width of the container. When `flex-direction` is column, the `flex` property applies to the height of the container.

## Input elements

The following selects all `input` elements that are not checkboxes and not radio buttons:

```css
input:not([type:checkbox]):not([type:radio]) {
    display: block;
    width: 100%;
    margin-top: 0;
}
```

For input elements, the width is determined by the size attribute, which is the number of characters it should contain without scrolling. Use the `width` attribute to force a specific width value.

## Nav styling

When creating menu items:
- Apply padding to the internal `<a>` tags to provide more clickable surface area.
- Use `display: block;`. This allows its parent to derive the height from their padding, not line height.

Horizontal nav styling:

```css
.ul-class {
    display: flex;
    margin: 0;
    padding: .5em;
    background-color: #5f4b44;
    list-style-type: none;
    border-radius: .2em;
}

.ul-class > li {
    margin-top: 0;    
}

.ul-class > li > a {
    display: block;
    padding: .5em 1em;
    background-color: #cc6b5a;
    color: #fff;
    text-decoration: none;
}

.ul-class > li + li {
    margin-left: 1.5em;
}
```


## Grid

Grid lets you create 2-D layouts. It works from the layout in.

Grid containers behave like block elements, it fills 100% of the available width.

You can use the `fr` unit, which stands for `fraction unit`. This is analogous to `flex-grow` in flexbox.

`grid-gap` defines the amount of space to add to the gutter between each grid cell. If you define this, it lies atop the grid lines.

### Terminology

- grid container: The element that you apply `display: grid` to
- grid item: A child of the grid container.
- grid line: The line between the columns and rows.
- grid cell: Area between grid lines.
- grid track: The space between two adjacent grid lines.
- grid area: The total space surrounded by four grid lines.

### Grid container properties

Apply the following properties to the grid container:

```css
.container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: min-content 1fr min-content;
    grid-template-areas:
        ". header     header    ."
        ". nav        nav       ."
        ". content    content   ."
        ". footer     footer    .";
    /* shorthand for *-columns, *-rows, and *-areas */
    grid-template: 
        [row1-start] "header header header" 25px [row1-end]
        [row2-start] "footer footer footer" 25px [row2-end];
    column-gap: 2rem;
    row-gap: 2rem;
    /* shorthand for row-gap and column-gap*/
    gap: 2rem 2rem;
    justify-items: start;
    align-items: center;
    /* align-items/justify-items */
    place-items: center;
    justify-content: space-around;
    align-content: end;
    /* shorthand for align-content and justify-content props */
    place-content: end space-around;
    /* size of any implicit grid tracks */
    grid-auto-columns: 60px 60px;
    /* places items in the grid automatically */
    grid-auto-flow: column;
    grid: 100px 300px / auto-flow 200px;
}
```

### Grid item properties

Apply the following properties to the grid items:

```css
.grid-item {
    grid-column-start: <number> | <name> | span <number> | span <name> | auto;
    grid-column-end: <number> | <name> | span <number> | span <name> | auto;
    grid-row-start: <number> | <name> | span <number> | span <name> | auto;
    grid-row-end: <number> | <name> | span <number> | span <name> | auto;
    /* shortnad for previous props */
    grid-column: <start-line> / <end-line> | <start-line> / span <value>;
    grid-row: <start-line> / <end-line> | <start-line> / span <value>;
    grid-area: header;
    justify-self: start;
    align-self: center;
    place-self: stretch;
}
```

### Sizing functions and keywords

- `fr`: fractional unit
- `min-content`: minimum size of the content.
- `max-content`: maximum size of the content.
- `minmax(min, max)`: sets the min and max for the element.
- `repeat(num, size)`: creates `num` rows or columns of `size` width

### Grid areas

If you don't want to count grid lines to place items, use grid areas. You apply `grid-template` on the grid container, and then the `grid-area` property on the child elements:

```css
.container {
    display: grid;
    grid-template-areas:
        "title  title"
        "nav    nav"
        "main   aside1"
        "main   aside2";
    grid-template-columns: 2fr 1fr;
    grid-template-rows: repeat(4, auto);
    grid-gap: 1.5em;
    max-width: 1080px;
    margin: 0 auto;
}

header {
    grid-area: title;
}

nav {
    grid-area: nav;
}
...
```

### Implicit grid

Implicit grid tracks have a size of `auto`, which means that they grow to the size of the grid item contents. Use `grid-auto-rows` or `grid-auto-columns` to set a size for implicit grid items.


# Positioning

Initially, every element has static positioning:

```css
position: static
```

Anytime you change the `position` value, the element becomes _positioned_. Positioned elements are removed from the document flow so that you can place them anywhere on the screen.

## Fixed positioning

Positions elements arbitrarily within the viewport (the _containing block_) using the following properties:

```
top:
bottom:
left:
right:
```

Fixed elements are removed from the document flow -- static elements display as if the fixed elements do not exist.

For static side navs, add a margin to the content to make sure that it doesn't flow behind it.

### Fixed example

An example of a `position: fixed` element is a modal. Here is the HTML:

```html
...
<div>
    <div class="modal" id="modal"></div>
    <div class="modal-backdrop"></div>
    <div class="modal-body">
        <button class="modal-close" id="close" />
        ... modal contents
    </div>
</div>
```

You position in in the viewport using the following:

```css
/* The modal-backdrop (grayed-out area behind the actual modal) */
/* The backdrop covers the entire viewport. */
.modal-backdrop {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.5);
}

/* Like the backdrop, the modal-body is positioned within the viewport */
.modal-body {
    position: fixed;
    top: 3em;
    right: 20%;
    bottom: 3em;
    left: 20%;
    background-color: #fff;
    /* allow scroll if necessary */
    overflow: auto; 
}
```

Use JS to grab the `modal` id, then use `display: none` or `display: block` to toggle it on or off.

## Absolute positioning

The location of absolute positioned elements is based on the closest positioned ancestor element. This ancestor element is its _containing block_.

Absolute positioning is used frequently with JS to build menus, tooltips, info boxes, etc.

### Absolute example

In this example, we will position a 'close' button for the modal described in the Fixed section. The 'close' icon looks like this:

```css
.modal-close {
    position: absolute;
    top: 0.3em;
    right: 0.3em;
    padding: 0.3em;
    cursor: pointer;
}
```

When the browser reads this `position: absolute`, it searches up the DOM heirarchy until it finds a positioned element and uses that as the positioning reference, or _containing block_.

## Relative positioning

Applying relative positioning does not impact the elements around the positioned elements. It moves the element relative to its original position in the document flow.

The main use for relative positioning is to create a containing block for an absolutely positioned element.

### Relative example

In the following example:
- `dropdown` is the relative-positioned containing block
- `dropdown-menu` is the absolute-positioned element that is hidden initially

```html
<div class="container">
    <nav>
        <div class="dropdown">
            <div class="dropdown-label">Main Menu</div>
            <div class="dropdown-menu">
                <ul class="submenu">
                    <li><a href="#">Home</a></li>
                    <li><a href="#">Coffees</a></li>
                    <li><a href="#">Brewers</a></li>
                    <li><a href="#">Specials</a></li>
                    <li><a href="#">About us</a></li>
                </ul>
            </div>
        </div>
    </nav>
</div>
```

The following CSS creates the positioned context:

```css
...
.dropdown {
    display: inline-block;
    position: relative;
}
...

.dropdown-menu {
    display: none;
    position: absolute;
    left: 0;
    top: 2.1em;
    min-width: 100%;
    background-color: #eee;
}

/* when you hover on dropdown, the menu displays. Toggle with JS */
.dropdown:hover > .dropdown-menu {
    display: block;
}
```

## Sticky positioning

This is a hybrid between relative and fixed positioning. The element scrolls until it reaches a specified spot in the viewport and then it 'sticks' in place.

You do not need a containing block for `sticky` positioning.


## Stacking contexts with z-index

When you remove an element from the document flow, you become responsible for all the things that the document flow normally does for you. This includes making sure that the positioned element does not: 
- Overflow outside the browser viewport and then hidden from the user
- Cover important content

When the browser parses HTML, it creates a _render tree_ that represents the appearance and position of each element. The browser paints elements in the order in which they are listed in the HTML. The position determines the order that the browser _paints_ the elements:
- First, it paints non-positioned elements.
- Next, it paints positioned elements using `z-index`.

Elements with a `z-index` establish a _stacking context_. A stacking context is a n element or a group of elements that are painted together with a browser. Elements with a higher `z-index` are positioned in front of elements with a lower `z-index`. Elements with a negative `z-index` are positioned behind static elements. No element outside the stacking context can be positioned between elements in the stacking context.

#### Best practice
Put your `z-index` values in variables:

```css
--z-one:    100;
--z-two:    200;
--z-three:  300;
--z-four:   400;
```



