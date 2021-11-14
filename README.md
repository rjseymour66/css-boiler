# Notes

CSS uses _late binding_: the content and its styles are not pulled together until after the authoring of both is complete.

The _root node_ is the ancestor of all elements in the document. It is `<html>`. `:root` is its special pseudo-class selector.

Define `line-height` on the body element so that it is inherited by rest of the document.

The original intention of vendor prefixes was to allow developers to experiment with technology before they used it in production. But developers started using it in production immediately. 

Use [Autoprefixer CSS online](https://autoprefixer.github.io/).

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
.container {
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
.item {
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

## Creating a nav

When creating menu items:
- Apply padding to the internal `<a>` tags to provide more clickable surface area.
- Use `display: block;`. This allows its parent to derive the height from their padding, not line height.
