# Demo: Using Flex-Order When One Item Is Extremely Large Causes Reflow

Given an HTML structure where we have a  like the following:

```
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
    order: 2; // Although the Content precedes the Sidebar in the DOM, make it visually appear second.
    background: #fefefe;
  }

  .sidebar-container {
    order: 1; // Although the Sidebar follows the content in the DOM, make it visually appear first.
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

https://compassionate-lalande-f41e7b.netlify.app/

