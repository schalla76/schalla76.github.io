---
layout: post
title: "Book reading notes"
date: 2019-09-01
categories: books
---

# CSS

## Source Links

[External link, Learn CSS Layout](http://learnlayout.com/toc.html)

[Youtube link Author: Derek Banas, CSS Tutorials](https://www.youtube.com/watch?v=CUxH_rWSI1k "CSS3 Tutorials")

[Youtube link Author: Derek Banas, CSS Tutorials](https://www.youtube.com/watch?v=mqLI2oN6rXQ)

[Udemy Course](https://www.udemy.com/course/css-the-complete-guide-incl-flexbox-grid-sass/)

## The text properties

- Color
- Background-Color
- Background-image
- Background: repeat
- Text-decoration:
  - underline
  - strike
  - bold
  - none
- Text-alin center
- Text-transform

## Font Properties

- Font-family
- Font-weight
- Font-height

## Hover states

- Hover
- Active

## List styles

- List-style:none,circle, disc
- Display:BLOCK,inline

## Border

- Border: WIDTH STYLE COLOR
- Border-right:
- Border-width
- Border-color
- Border-style
- Border-radius

## Box-sizing

Determines the width and the height of the element. Default all the element have content-box. We can set it to border-box. The box-sizing is not inherited, so we may have to use global selector.

## Response design attributes

- Width-min
- Width-max

## Display attribute

- Display: block
  - Takes entire width
  - Puts line breaks
  - Example div
- Display: None
  - Removes it from the flow. There is no blank space for that element.
- Display: inline,
  - Doesn't have height and width
  - No new line
- Display: inline-block
  - No new line
  - Has padding-top and padding-bottom
  - Also takes the space in html
- Display: hidden
  - Its hidden but there is blank space.

## CSS selectors

### Types of selectors

- Tag selectors
  - ex: h1
- Class selectors
  - ex: .blog-post
- Universal selectors
  - ex: \*
- ID selectors
  - ex: #main-container
- Attribute selector
  - ex: [disabled]

### Selector combinations

#### Types of combinators

- Adjacent siblings
  - ex: div + p {}
  - The should be an immediate sibling.
- General siblings
  - ex: div ~ p {}
  - The siblings may not be the immediate siblings
- Child
  - ex: div > p {}
  - Should be direct child
- Descendent
  - ex1: div p {}
  - ex2: #main-container h2
  - May not be direct child
- Attribute selector
  - ex: [attribute] {};
  - ex: [attribute="value"]{};
  - ex: [attribute~="value"]{}; //List attribute

#### (Tag and Class) or (Tag and ID) or (Class and Class) combination

- The tag and class are specified without spaces then the tag with that particular class is selected. If there is a space then the tags child with that particular class is selected.

#### Grouping by comma

- We can group multiple Tags with comma and apply th same style.

### Selector specificity

- The specificity order is:
  - Inline
  - ID
  - Class, Attribute
  - Tags
  - Global
  - Inherent

## CSS Value types

- Pre-defined Options
  - ex: display: block
- Colors
  - ex: background: red;
- Length, sizes and order
  - height: 10px;
- Functions
  - transform: scale(...);

## Box model

- Margin
- Border
- Padding
- Width
- Height

### Width & Height

If width is 100% then it occupies the full width of the parent. The block element have 100% width by default.
If height is 100% then the parent should also 100%.

## Margin collapsing

- The adjustment elements margin is collapsed and the largest wins

## CSS functions

- Calc - calculates
  - ex: calc(100% - 40)
- url -
  - ex: background: url("...")

## Pseudo Classes

Pseudo classes are prefix by colon, a define style on state

Examples: :hover, :active, :invalid etc

## Pseudo Element

Pseudo element allows you to style part of the element, it is prefix by double colon.

Examples: ::after, ::before, ::first-letter

## !important and not

Important overrides all the specificity and all other selectors

ex: h1 { color: red !important;}

not negates

ex: :not(p) { color: green; }

## Alignments

[source](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Alignment)

- text-align
- vertical-align
- align-item
- justify-item
- align-content
- justify-content
- align-self
- justify-self

margin: auto : aligns item horizontally

## Colors

- #ffffff - white
- rgb(255, 255, 255) - white
- rgba(255, 255, 255, 1) - opaque
- #ccc - Grey
- #444 - Drak Grey

## box-shadow

[source](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow)

## outline

The outline is shown on focus. This can be remove by below:

```css
.button-class:focus {
  outline: none;
}
```

## Things to remember

- Some of the styles are directly assigned by the browser. Such as the button font, anchor text color etc
- To inherit these properties you need explicitly specify inherit or override the style.

## Floats

- right
- left

### Clear fix

## Positioning

**Static, Relative, Absolute, Inhertance, Fixed, Sticky**

- Static: This is default - normal document flow. With static position we can't change the position by setting the top, bottom etc properties

- Fixed: When fixed is applied the element is removed form the document flow, same as absolute. The block level element becomes inline-block element. The fixed position is relative to the view port (not body or not html or not parent).

- Relative: This is relative to itself (relative to its default position).

- Absolute: This is removed from document flow (i.e. that means for other elements this relative is not there in document) and is relative to the first non static ancestor. To make it relative to parent set parent position to relative.

- Sticky: It is hybrid of fixed and relative. It is position relative to the view port. The element is not removed from the document flow. It behaves like fixed when the page is scrolled and element touches the viewport. It only behaves like fix until the parent is visible.

The final piece of information to remember is that both relatively and absolutely positioned items won’t affect the static and fixed items around them (absolutely positioned items are removed from the flow, relatively positioned items occupy their original position).

### Difference and similarities

| Fixed                             | Obsolute                                    | Similar           |
| --------------------------------- | ------------------------------------------- | ----------------- |
| Position is relative to view port | Position is relative to non static ancestor | Removed from flow |
| Used for navigation bar           | Used for element anchored item              |                   |

| Fixed                             | Relative                                                        | Similar |
| --------------------------------- | --------------------------------------------------------------- | ------- |
| Position is relative to view port | Position is relative to self                                    |         |
| Removed from the flow             | Not removed from flow                                           |         |
| Used for navigation bar           | Used in conjunction with obsolute position                      |         |
|                                   | Set Overflow hidden to clip children out of parent's boundaries |         |

## z-index

- This doesn't have any effect if the position is static. Only exception is flex-box
- The default z-index is auto (which is zero)
- If elements have same z-index then they are displayed in the order they are placed in the page.

## overflow: hidden

- Specifies the behavior of the child elements when they are out of parent boundaries
- Overflow hidden on the body is passed to html. To make it work on the body set the html overflow to hidden or auto

## images and background

### Background

example:

```css
background-image: url

background-color: green

background-repeat: no-repeat // default is repeat
background-repeat: repeat-x // repeats x axis
background-size: 300px; //When only width is specified then aspect ratio is maintained

background-size: 100%; // This may crop the height of the image to make the width 100%

background-size: cover; // Cover would fill in all the white space by zoom in or zoom out based on the img is landscape or portrait.'

background-size: contain; This may leave white space but does not crop the image.


background-position: x y; x, y can be %. This determines how much % is cropped on x, y.

background-position: left x bottom y; x is cropped on left, y is cropped on bottom.

background: center; centers the image
```

background-clip and background-origin: This are used to defined how the image is clipped with border and padding.

background-attachment: Used to define the scrolling of the background image.

background shorthand:

background: imageorcolor position/size no-repeat origin clip attachment

The background shorthand overrides with the default for the properties that are not specified.

### Images

The image containers width and height doesn't effect the img tags width and height

When image width or height is set to 100% then if the container is block or inline-block then the img is resized to the container. If the container is inline then this doesn't work.

Images doesn't have as many options as background. The advantage of using images is the accessibility.

To remove the white space at the bottom of image set the vertical alignment to top.

## Gradient

For gradience the background with function is used

background: linear-gradient(start, color);
background: radiant-gradient(start, color);

start can be to top/to bottom or degrees. In color could be multiple colors and also specify the end.

## Filters

Filters are css functions to manipulate the look of the item.
example: Blur(), Grayscale()

## SVG - Scalable Vector Graphics

## Units

### Types of units

- Pixes
- Percentages
- rem and em
- vw and vh

#### Percentages

Percentages are based on the container element. The container element depends on the position. For the positions fixed and absolute the container becomes view port and first non static ancestor respectively.Also for absolute the parent content and padding is consider to calculate the percent. For the relative and static the parent is the closest block level ancestor and only the content of the ancestor is considered for calculating.

- 100% height won't work for the relative item. To make it work we need to make body and html height to 100%
- The percentage units are used for widht and height

#### rem, em

- This units are mostly used for fonts.
- The rem is based on the root em font size but em is based on the parent em.
- One rem = x times the root size px
- We can use rem for fonts, padding, margin, width, height
- Don't use rem for box=shadow, border.

#### view port

- vw, vh refer to view port width and height
- vmax refer to the width or height, whichever is max
- vmin refer to the width or height, whichever is min
- view port displays scroll bars on windows. To avoid scroll bars use vw/vh 100% and for body over-flow-x: hidden/over-flow-y: hidden
- This units are mostly used for width and height

#### Hardware pixels vs Software pixels

Hardware pixels are the actual pixels of the device, for example the iphone has denser pixel ratio. When the web page is displayed without any viewport meta tag then the browser just considers the hardware pixel ratio.

- veiwport meta tag

```css
<metaname="viewport"content="width=device-width, initial-scale=1.0">viewportattributes: maximum-scale, minimum-scale,
  user-scalable;
```

#### Media queries

@media(condition){
style
}

Example:

```css
@media (min-width: 40rem) {
  .className {
    font-size: 3 rem;
  }
}
```

## Styling Forms

- input element when set to display:block won't occupy the complete width. We need to set width to 100% for them to occupy the complete width.

## Browser specific style

- The browser provides some default styles. The chrome styles start with -webkit, the firefox styles start with -moz

## Form validation types

- :invalid, :valid // Does validation for email etc
- required // Mandatory field validation
- readonly // Readonly field

## cursor

ex: h1 { cursor: not-allowed}

## Fonts

- Generic family fonts are serif, sans-serif, fixed-width, these are generic and each will point to specific font in the browser defaults.
- If no fonts are specified then browser standard font is selected.
- Web fonts are the fonts where you specify the url to the font
- Fonts can be imported into the css file using import
  - example: @import(url);
- Fonts can also be include in the website using ttf files

```css
@font-face {
  font-family: "Roboto";
  src: url("../fonts/Roboto-Regular.ttf");
}
```

### Font formats

- ttf - True Type Font
- woff - Web Open Font Format

### Font properties

```css
font-size: 1.2rem;
font-weight: 400;
font-style: italic;
font-variant: small-caps;
font-stretch: ultra-condensed;
letter-spacing: 0.5 rem;
white-space: pre; //pre, pre-wrap, pre-line, nowrap, normal
line-height: 2;
text-decoration: underline dotted red; //underline, overline, line-through, wavy, none
text-shadow: x y blur color;
```

- Font shorthand
  - font: font-style font-variant font-weight font-size/line-height font-family //size and family are required
  - example: font: italic small-caps 700 1.2rem/2 "Roboto", sans-serif;

## Flex box

- display: flex
- By default flex has:
  - flex-direction:row;
  - flex-wrap: nowrap;

### Flex properties

- flex-direction: row/column/row-reverse/column-reverse;
- flex-wrap: nowrap/wrap/wrap-reverse;
- To align items to left and right we can use align-items:space-between

### Flex main axis and cross axis

- For flex-direction: row the main axis is from left to right or right to left based on the row/row-reverse
- For flex-direction: colum the main axis is from top to bottom or bottom to top based on the column/column-reverse
- The align-items aligns items on the cross axis
- The justify-content align items on the main axis
- The align-content allows to justify the items on the cross axis if the items wrap
- The align-self allows to align individual item on the cross axis

### Flex order

- The order Positions the items on the main axis. By default the order for all items is 0. The items are positioned in ascending order i.e -1 sets item to the starting...

### Flex grow and shrink

- These are similar to the fr units

### Flex basis

- This sets the width or height based on the main axis. If the flex-directions is row then it sets/overrides the width. if the flex-direction is column then it sets/overrides the height.

## Grid

- By default grid creates a single column and places all the items in rows
- grid-template-columns - Defines the columns
- grid-template-rows - Defines the rows
- grid-column-start, grid-column-end - Defines the item column start position
- grid-row-start, grid-row-start - Defines the item row and end position
