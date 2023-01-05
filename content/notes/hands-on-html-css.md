---
title: "Hands on HTML & CSS"
tags:
- HTML
- CSS
weight: -5

---

# Hands on HTML & CSS



> This is my practices on HTML&CSS in order to develop the front-end of websites.

## References

[MDN HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML)

[HTML basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)

[CSS basics](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics)



## HTML Basics

### HTML element

A HTML document describes the content of a web page, which contains many [HTML element](https://developer.mozilla.org/zh-CN/docs/Glossary/Element)s:

![Example: in <p class="nice">Hello world!</p>, '<p class="nice">' is an opening tag, 'class="nice"' is an attribute and its value, 'Hello world!' is enclosed text content, and '</p>' is a closing tag.](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/anatomy-of-an-html-element.png)



### Basic HTML

A basic `index.html` is:

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>My test html page</title>
</head>
<body>
    <p><strong>HTML tag: h1~h5</strong></p>
    <h1>HTML Basics</h1>
    <h2>2023年，崭新的一年</h2>
    <h3>2023年，崭新的一年</h3>
    <!-- <h4>2023年，崭新的一年</h4>
    <h5>2023年，崭新的一年</h5>
    <h6>2023年，崭新的一年</h6> -->

    <p><strong>HTML tag: img</strong></p>
    <img src="images/2023.jpg" alt="my test image" width="600" height="400">

    <p><strong>HTML tag: p</strong></p>
    <p>新的转机和闪闪的星斗，正在缀满没有遮拦的天空。这是5000年的象形文字，这是未来人们凝视的眼睛👁。</p>

    <p><strong>HTML tag: strong</strong></p>
    <p>新的转机和闪闪的星斗，正在缀满没有遮拦的天空。这是5000年的象形文字，<strong>这是未来人们凝视的眼睛👁。</strong></p>

    <p><strong>HTML tag: Unordered List</strong></p>
    <p>At Mozilla, we're a global community of</p>
    <ul>
        <li>technologists</li>
        <li>thinkers</li>
        <li>builders</li>
    </ul>
    <p>working together…</p>

    <p><strong>HTML tag: Ordered List</strong></p>
    <p>At Mozilla, we're a global community of</p>
    <ol>
        <li>technologists</li>
        <li>thinkers</li>
        <li>builders</li>
    </ol>
    <p>working together…</p>

    <p><strong>HTML tag: a</strong></p>
    <a href="https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/HTML_basics">mdn HTML Basics</a>
</body>
```



The page based on it:

![e3ed2a84-b999-4113-b138-310a22c9ab35](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/e3ed2a84-b999-4113-b138-310a22c9ab35.jpeg) 

## CSS Basics

[CSS basics](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics)

CSS is all about `style` (how it looks) of web content. It concerns the following questions:

* How do I make text red? 
* How do I make content display at a certain location in the (webpage) layout? 
* How do I decorate my webpage with background images and colors?



### File Layout

```shell
$ tree -L 2
.
├── images
│   └── 2023.jpg
├── index.html
└── styles
    └── style.css
```

We store the CSS content in a `.css` file under the `styles` folder. The content of `style.css` is like:

```css
p {
    color: red;
}
```

and imports it in `index.html`:

```html
<head>
    <meta charset="UTF-8">
    <title>My test html page</title>
  
    <!-- import css here -->
    <link href="styles/style.css" rel="stylesheet"> 
</head
```

which makes all the `<p>` elements red, as following:

![image-20230105145429781](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20230105145429781.png)



### CSS Rulesets

The basic element of CSS is called `ruleset`, which is explained as following:

![CSS p declaration color red](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/css-declaration-small.png)

* SelectorIt defines the element(s) to be styled (in this example, `<p>` elements). To style a different element, change the selector.

* Declaration. It specifies which of the element's **properties** you want to style, like `color: red;`.



You can change the style:

```css
p {
    color: blue;
    width: 500px;
    border: 1px solid black;
  }
```



![image-20230105160938495](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20230105160938495.png)

You can select multiple elements using `,`:

```css
p,
li,
h1 {
  color: red;
}
```

which generates:

![image-20230105161248652](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20230105161248652.png)

### Different Types of Selectors

There are 5 basic types of selectors.

| Selector name                                              | What does it select                                          | Example                                                      |
| :--------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Element selector (sometimes called a tag or type selector) | All HTML elements of the specified type.                     | `p` selects `<p>`                                            |
| ID selector                                                | The element on the page with the specified ID. On a given HTML page, each id value should be unique. | `#my-id` selects `<p id="my-id">` or `<a id="my-id">`        |
| Class selector                                             | The element(s) on the page with the specified class. Multiple instances of the same class can appear on a page. | `.my-class` selects `<p class="my-class">` and `<a class="my-class">` |
| Attribute selector                                         | The element(s) on the page with the specified attribute.     | `img[src]` selects `<img src="myimage.png">` but not `<img>` |
| Pseudo-class selector                                      | The specified element(s), but only when in the specified state. (For example, when a cursor hovers over a link.) | `a:hover` selects `<a>`, but only when the mouse pointer is hovering over the link. |

### Fonts and Text

First, find the [output from Google Fonts](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/What_will_your_website_look_like#font) that you previously saved from [What will your website look like?](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/What_will_your_website_look_like). Add the `link` element somewhere inside your `index.html`'s head (anywhere between the `<head>` tags). It looks something like this:

```html
<link href="https://fonts.googleapis.com/css?family=Open+Sans" rel="stylesheet" />
```



Next, delete the existing rule you have in your `style.css` file. It was a good test, but let's not continue with lots of red text.



Add the following lines (shown below), replacing the `font-family` assignment with your `font-family` selection. The property `font-family` refers to the font(s) you want to use for text. This rule defines a global base font and font size for the whole page. Since `<html>` is the parent element of the whole page, all elements inside it inherit the same `font-size` and `font-family`.

```css
html {
  font-size: 10px; /* px means "pixels": the base font size is now 10 pixels high */
  font-family: "Open Sans", sans-serif; /* this should be the rest of the output you got from Google Fonts */
}
```



The page is like this now:

![image-20230105164921181](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20230105164921181.png)



Now let's set font sizes for elements that will have text inside the HTML body (`<h1>`, `<li>` and `<p>`). We'll also center the heading. Finally, let's expand the second ruleset (below) with settings for line height and letter spacing to make body content more readable.

```css
h1 {
  font-size: 60px;
  text-align: center;
}

p,
li {
  font-size: 16px;
  line-height: 2;
  letter-spacing: 1px;
}
```

which changes the font-size and other styles:

![image-20230105165727652](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20230105165727652.png)



### The Box Model

Most HTML elements can be thought of as boxes sitting on the top of other boxes. CSS is about setting the size, color and position of boxes.

![Three boxes sat inside one another. From outside to in they are labelled margin, border and padding](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics/box-model.png)

 Each box taking up space on your page has properties like:

- `padding`, the space around the content. In the example below, it is the space around the paragraph text.
- `border`, the solid line that is just outside the padding.
- `margin`, the space around the outside of the border.

In this section we also use:

- `width` (of an element).
- `background-color`, the color behind an element's content and padding.
- `color`, the color of an element's content (usually text).
- `text-shadow` sets a drop shadow on the text inside an element.
- `display` sets the display mode of an element. (keep reading to learn more)



### Changing the Page Color

```css
html {
  background-color: #00539f;
}
```

### Styling the Body

```css
body {
  width: 600px;
  margin: 0 auto;
  background-color: #ff9500;
  padding: 0 20px 20px 20px;
  border: 5px solid black;
}
```

- `width: 600px;` This forces the body to always be 600 pixels wide.
- `margin: 0 auto;` When you set two values on a property like `margin` or `padding`, the first value affects the element's top *and* bottom side (setting it to `0` in this case); the second value affects the left *and* right side. (Here, `auto` is a special value that divides the available horizontal space evenly between left and right). You can also use one, two, three, or four values, as documented in [Margin Syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/margin#syntax).
- `background-color: #FF9500;` This sets the element's background color. This project uses a reddish orange for the body background color, as opposed to dark blue for the [``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html) element. (Feel free to experiment.)
- `padding: 0 20px 20px 20px;` This sets four values for padding. The goal is to put some space around the content. In this example, there is no padding on the top of the body, and 20 pixels on the right, bottom and left. The values set **top, right, bottom, left**, in that order. As with `margin`, you can use one, two, three, or four values, as documented in [Padding Syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/padding#syntax).
- `border: 5px solid black;` This sets values for the width, style and color of the border. In this case, it's a five-pixel–wide, solid black border, on all sides of the body.

### Positioning and Styling the Main Page Title

```css
h1 {
  margin: 0;
  padding: 20px 0;
  color: #00539f;
  text-shadow: 3px 3px 1px black;
}
```

`text-shadow` applies a shadow to the text content of the element. Its four values are:

- The first pixel value sets the **horizontal offset** of the shadow from the text: how far it moves across.
- The second pixel value sets the **vertical offset** of the shadow from the text: how far it moves down.
- The third pixel value sets the **blur radius** of the shadow. A larger value produces a more fuzzy-looking shadow.
- The fourth value sets the base color of the shadow.

### Centering the Image

```css
img {
  display: block;
  margin: 0 auto;
}
```

Next, we center the image to make it look better. We could use the `margin: 0 auto` trick again as we did for the body. But there are differences that require an additional setting to make the CSS work.

The `body` is a **block** element, meaning it takes up space on the page. The margin applied to a block element will be respected by other elements on the page. In contrast, images are **inline** elements, for the auto margin trick to work on this image, we must give it block-level behavior using `display: block;`



Now, the page looks like this:

![image-20230105174108098](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20230105174108098.png)
