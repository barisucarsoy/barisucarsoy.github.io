---
layout: default
title: "Exploring Markdown and a Nod to React"
date: 2025-05-06
categories: [trial]
---

## Mastering Markdown

Markdown is a lightweight markup language with plain-text-formatting syntax. It's designed so that it can be converted to HTML and many other formats. Here are some common features:

### Text Formatting

You can make text **bold** by surrounding it with double asterisks or double underscores, like `**bold**` or `__bold__`.
For *italic* text, use single asterisks or underscores: `*italic*` or `_italic_`.
You can combine them for ***bold and italic*** text: `***bold and italic***`.
Strikethrough uses two tildes: `~~strikethrough~~`.

### Headings

You've already seen `##` for H2. Markdown supports headings from H1 to H6:

```markdown
# This is an H1
## This is an H2
### This is an H3
#### This is an H4
##### This is an H5
###### This is an H6
```

### Lists

#### Unordered Lists
Create unordered lists using asterisks, plus signs, or hyphens:

```markdown
* Item 1
* Item 2
  * Sub-item 2a
  * Sub-item 2b
+ Item 3
- Item 4
```

* Item 1
* Item 2
  * Sub-item 2a
  * Sub-item 2b
+ Item 3
- Item 4

#### Ordered Lists
Create ordered lists using numbers followed by periods:

```markdown
1. First item
2. Second item
3. Third item
   1. Indented item
   2. Another indented item
```

1. First item
2. Second item
3. Third item
   1. Indented item
   2. Another indented item

### Links

You can create an inline link like this: `[Visit GitHub](https://github.com)`.
Result: [Visit GitHub](https://github.com)

### Images

The syntax for images is similar to links but with a preceding exclamation mark:
`![Alt text for image](/assets/images/your-image.jpg "Optional Title")`

*(Note: For this to work, you'd need to place `your-image.jpg` in an `assets/images` directory in your repository.)*

### Code Blocks

#### Inline Code
Wrap inline code with backticks: `let variable = "hello";`.

#### Fenced Code Blocks
For longer blocks of code, use triple backticks. You can also specify the language for syntax highlighting:

```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet("Developer");
```

```python
def add(a, b):
  return a + b

print(add(5, 3))
```

```html
<div>
  <p>This is some HTML.</p>
</div>
```

### Blockquotes

To create a blockquote, use the `>` character:

> This is a blockquote. It's often used for quoting text from another source.
>
> Second paragraph in the blockquote.

### Horizontal Rule

Create a horizontal rule using three or more hyphens, asterisks, or underscores:

---
***
___

## Thoughts on Including React Components

Integrating dynamic React components directly into Markdown files processed by a standard Jekyll setup can be a bit tricky, as Jekyll primarily converts Markdown to static HTML.

However, here are a few conceptual approaches:

1.  **Static HTML Export from React**:
    If your React component doesn't require a lot of client-side interactivity after the initial render, you could pre-render it to static HTML and paste that HTML into your Markdown file. This isn't truly "React" at runtime but can be useful for simpler components.

2.  **Web Components**:
    You could wrap your React component as a Web Component. Jekyll can output the HTML tag for the web component, and you would include the JavaScript bundle for the web component in your site's assets.

3.  **Client-Side Hydration**:
    *   Write your React component as usual.
    *   Bundle your React app using a tool like Webpack or Parcel. This will produce a JavaScript file (e.g., `bundle.js`).
    *   Include this `bundle.js` in your Jekyll site's `<head>` or before the closing `</body>` tag (e.g., in your `_layouts/default.html`).
    *   In your Markdown file (which will be converted to HTML), include a placeholder `div` with a specific ID:
        ```html
        <div id="react-app-goes-here"></div>
        ```
    *   Your React JavaScript bundle would then target this `div` to mount your component:
        ```javascript
        // In your React app's entry point (e.g., index.js or app.js)
        import React from 'react';
        import ReactDOM from 'react-dom';
        import MyComponent from './MyComponent'; // Your React component

        const container = document.getElementById('react-app-goes-here');
        if (container) {
          ReactDOM.render(<MyComponent />, container);
        }
        ```
    This approach means Jekyll handles the static site generation, and React takes over a specific part of the page on the client-side. You'd need a build process for your React code separate from Jekyll's build process.

4.  **Using a `<script>` tag for simple cases**:
    For very simple, self-contained React snippets, you could technically use `<script type="text/babel">` and include the Babel standalone transpiler along with React and ReactDOM via CDNs. This is generally not recommended for production but can work for quick demos.

    ```html
    <div id="like_button_container"></div>

    <!-- Load React. -->
    <!-- Note: when deploying, replace "development.js" with "production.min.js". -->
    <script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>


    <!-- Load our React component. -->
    <script type="text/babel">
      'use strict';

      const e = React.createElement;

      class LikeButton extends React.Component {
        constructor(props) {
          super(props);
          this.state = { liked: false };
        }

        render() {
          if (this.state.liked) {
            return 'You liked this.';
          }

          return e(
            'button',
            { onClick: () => this.setState({ liked: true }) },
            'Like'
          );
        }
      }

      const domContainer = document.querySelector('#like_button_container');
      ReactDOM.render(e(LikeButton), domContainer);
    </script>
    ```
    You would place the HTML part (`<div id="like_button_container"></div>`) in your Markdown, and the script tags ideally in your layout file or an includes file so they are loaded on the page.

For a simple blog, sticking to Markdown's strengths is often the easiest path. If you need rich interactivity, the client-side hydration (option 3) is a common pattern, but it adds complexity to your build and deployment.

Happy blogging and coding!
