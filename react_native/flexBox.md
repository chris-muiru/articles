## Intro

Today is actually a day i enjoyed. I gained the knowledge on flexbox in react native. according to the documentation **flex** defines how items are going to fill over available space along main axis. In react native,the main axis is the column.

```
MAIN - flex:1
   item-1:flex-1
   item-2:flex-2
   item-3:flex-3

1+2+3=6
item-1 - 1/6
item-2 - 2/6
item-3 - 3/6
```

-  `flexDirection` - column,row,row-reverse
-  `layoutDirection` - LTR,RTL
-  `justifyContent` - align along main axis - flex-start,flex-end,center
-  `alignItems` - along cross axis.
-  `alignSelf` - individual item.
-  `alignContent` - flexWrap must be present. defines distribution along cross-axis.
-  `flexWrap`
-  `flexBasis` - axis independ way of providing the size of an item. if direction=column,height else width.
-  `flexGrow` - how item grows in relation to siblings. If 2,grows 2 times larger than siblings.
-  `flexShrink`

## position

`position` : absolute | relative.

## size

-  `width|height`:'100| "10%" | auto'
