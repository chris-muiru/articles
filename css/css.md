##  z-index
- `z-index` - specifies the stack order of an element(which element should be placed in front of or behind the other)
- an element can have a +ve or -ve stack order
- it only works on positioned elements(absolute,relative,...) and flex items
```css
img {
  position: absolute;
  left: 0px;
  top: 0px;
  z-index: -1;
}
<img src="img_tree.png">
<p>Because the image has a z-index of -1, it will be placed behind the text.</p>
```
## overflow
specifies wheter to clip the content or add scrollbars when content of an element is too big to fit in specified area.
### values 
- `visible` - overflow not clipped.(default)
- `hidden`   
-  `scroll`
-  `auto`- similar to scroll but adds  scrollbar when necessary. 

only works for block elements with specified heights 

## Combinators 
explains the rship between selectors.
### types 
- descendant selector (space)
```css
 div p {
  background-color: yellow;
}
```
-    child selector (>) - The child selector selects all elements that are the children of a specified element.
```css
div > p {
  background-color: yellow;
}
```
-    adjacent sibling selector (+)
The adjacent sibling selector is used to select an element that is directly after another specific element.

Sibling elements must have the same parent element, and "adjacent" means "immediately following".

The following example selects the first `<p>` element that are placed immediately after `<div>` elements:
```css
div + p {
  background-color: yellow;
}

```
-    general sibling selector (~)
The general sibling selector selects all elements that are next siblings of a specified element.
```css
div ~ p {
  background-color: yellow;
}
```
