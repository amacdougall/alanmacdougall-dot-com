---
weight: 30
title: "Quick Block Uncommenting"
---

# Quick Block Uncommenting

A quick and easy way of commenting or uncommenting large blocks of code in
languages with multiline comments, such as the `/* wing comments */` found in
C-like languages:

```
/* DEBUG: display complicated logging data
$.log("Baking.Cookies.recipe.build:",
  "recipe_id => ", recipe_id,
  "; recipe_contents => ", $.extend(true, {}, recipe), // deep clone
  "; options => ", options
);
// */
```

Right now the whole block is commented out; but when you cap the top wing
comment, you get this:

```
/* DEBUG: display complicated logging data */
$.log("Baking.Cookies.recipe.build:",
  "recipe_id => ", recipe_id,
  "; recipe_contents => ", $.extend(true, {}, recipe), // deep clone
  "; options => ", options
);
// */
```

Your code editor certainly provides a way of toggling comments, but over large
code blocks this may involve making a large selection first. With this
technique, adding or removing just two bytes can toggle comments on a block of
any length.
