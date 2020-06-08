# Code style guide 

## Web(like) code: HTML, CSS/SCSS

Heavily based on [codeguide.co](https://codeguide.co), with the additions/exceptions below:


### CSS properties declaration order

based on: https://codeguide.co/#css-declaration-order

with the "outside-in" ordering of properties within a the box model group, meaning, the order (top-bottom):  
1. size (width, height, max-width, etc)
2. margins
3. borders
4. paddings
5. display

### HTML multiple classes order
And apply the same ordering principle for HTML utility-like classes. For example:
> GOOD
```
<div class="w-100 m-3 b-1 p-3 d-flex align-items-center font-weight-bold text-warning bg-light">content here</div>
```

> BAD
```
<div class="font-weight-bold text-warning d-flex align-items-center w-100 bg-light m-3 p-3">content here</div>
```
