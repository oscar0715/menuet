---
title: Introduction to CSS3
tags:
  - CSS
categories:
  - Technology
  - Web
date: 2016-09-19 21:40:25
---
Coursera

Introduction to CSS3
<!-- more -->

***

# Cascading Style Sheet
Order:
1. Browser Default
1. External Style Sheet
1. Internal Style Sheet
1. Inline Style

Rule precedence
1. The rule from the most recent file
2. `!important`

# Color
1. Avoid color name(different browser may different)
2. hexadecimal #0000FF
3. RGB (0,0,1) 

Check Color
wave.waveaim.org

# Text
1. font-family
    - Use sans serif font
2. font-style
    - normal, italic, oblique
3. font-variant
    - normal, small-caps
4. font-size
    - pixel
    - percentage
5. color
6. background-color
7. text-align
8. line-height

# Display
1. inline:
2. block: force line break (width and height)
3. inline-block (inline and accept width and height)
4. overflow: visible/hidden/scroll/auto
5. display:table / display:table-cell
6. clear: not allow other flow element

# Link
a:link    normal
a:visited
a:hover    by mouse
a:focus    activeted with the keyboard
a:active    click

# List
list-style-type
list-style-image

# Advanced Selector
1. Descendant selectors (nav a)
2. child selectots (nav > a)  more specific, immediate child
3. #ID selector (#id{})
4. class selector (.class{})

conflict:
most recently uncless "!important"

1. select by attribute 
a[href^='http://umich']   begin with
a[href$='.png']   b=end with
a[href*^*='umich']   wild card

# Browser Capability
Not all browser support html5/CSS3
http://caniuse.com

Prefixs:
- gradient 
- border-radius

# Pesudo Classes and Elements
`a:hover{}`

Classes:
1. Lind:
    - :link 
    - :visited 
2. User Action: 
    - :hover
    - :active
    - :focus
3. Forms: 
    - :enabled
    - :checked
    - :disabled

Elements:
1. Textual
    - :first-letter
    - :first-line
2. Positional / Generated
    - :before
    - :after
3. Fragments
    - ::selection

# Transition
Properties
1. transition-property
2. transition-duration
3. transition-timing
4. transition-delay

# Position
Four position properties:
1. static
    - default
    - next available position
2. reletive
    - not affect others 
    - used as container for other elements
3. absolute 
    - one element can be on top of another element
4. fixed


# 学习笔记

## flex box
`<button>` 和 `<button>` 之间会莫名其妙多出一点空间。如果用 flex box 就不会有这些空间。

例如 button 的父元素的 class 是 key_board
``` css
#key_board {
    /*
        https://css-tricks.com/fighting-the-space-between-inline-block-elements/
        flex box remove ugly
        line between button
    */
    display: -webkit-flex;  

    /*
        https://drafts.csswg.org/css-flexbox/#flex-wrap-property
        allow multiple Flex Lines 
    */
    flex-flow: row wrap; 
}
```

## 第一个子元素 margin-top 属性传递给父元素的问题
http://www.programgo.com/article/9694138194/
    
父元素加
`overflow:hidden;`


## 隐藏某个元素
`visibility: hidden;`

## 圆角
`border-radius: 10px;`

## 阴影
`box-shadow: 3px 3px 2px #888888;`

## 鼠标移动到div变成一只手
``` css
selector {
    cursor:pointer;
    cursor:hand;
}
```

## 有些属性要加前缀，这里有个指南
http://shouldiprefix.com/
