# Demo: Using Flex-Order When One Item Is Extremely Large Causes Reflow

https://compassionate-lalande-f41e7b.netlify.app/

Given an HTML structure where we have a large (~200kB gzip) flex item
(`.content-container`) adjacent to a relatively small flex item
(`.sidebar-container`), attempting to load the larger item first by placing it
in the DOM first and using flex order to rearrange the visual order can cause
reflow issues:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Demo: Using Flex-Order When One Item Is Extremely Large Causes Reflow</title>
</head>
<style>
  .flex-container {
    display: flex;
  }

  .content-container {
    /* Although the Content precedes the Sidebar in the DOM, make it visually appear second. */
    order: 2; 
    background: #fefefe;
  }

  .sidebar-container {
    /* Although the Sidebar follows the content in the DOM, make it visually appear first. */
    order: 1;
    background: red;
    width: 180px;
  }
</style>
<body>
  <div class="flex-container">
    <div class="content-container">
      <h1>Content (~200kB)</h1>
      ...
    </div>
    <div class="sidebar-container">
      <h2>Sidebar</h2>
    </div>
  </div>
</body>
</html>
```

## Why does this happen?

Browsers don't need to wait to download the entire HTML before performing a
first paint and instead will try to paint based on what has already arrived.
Because the content bytes are the bulk of what the browser will initially
download, it will try to paint this before even knowing that a sidebar exists as
a flex item sibling to the content and will therefore not allocate space for it.
When the sidebar bytes do get transferred, it performs a reflow correcting this
mistake.

[1] https://www.tunetheweb.com/blog/critical-resources-and-the-first-14kb/


