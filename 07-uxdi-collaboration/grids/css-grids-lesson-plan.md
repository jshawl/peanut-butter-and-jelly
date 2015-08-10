# CSS Grids

## Learning Objectives
* Explain the benefits of using CSS grids in web design.
* Identify the basic components of a grid.
* Review float, clearfix and other CSS properties.
* Build a grid from scratch.
* Use nested columns in a grid.
* Examine how grids are utilized in a front-end framework (e.g., Bootstrap).

## Opening Exercise

Whiteboard a wireframe for [TBD website].
* Focus on the site's overall structure and main components.
* Keep an eye for width, height, proportion, number of components.

## Why use a CSS grid?

#### Structure
* Debugging CSS is a PAIN. It's easy to start throwing CSS selectors and properties at a problem, but once you get in too deep, it's hard to dig your way back out.
* Grids aren't a cure-all to CSS woes, but they give your site a form and structure to work from.

## Basic components of a grid

#### Rows
* The highest-level component of a grid.
* Comprised of columns.
* [Show where rows are on an example website]

#### Columns
* Contain and separate site content.
* [Show where columns are on an example website].


## Let's build a grid

You don't need a fancy-schmancy front-end framework to reap the benefits of a CSS grid. Let's start building one from scratch.

#### Create HTML document

In your in-class folder, create a blank HTML file called `index.html`
* In the `<head>`, let's link to a stylesheet called `style.css`.
* And don't forget to create that stylesheet: `$ touch style.css`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <link rel=stylesheet href="style.css" type="text/css">
</head>
<body>
</body>
</html>
```

#### Define column and row selectors

Let's start by creating two `.column` and `.row` class selectors.
* These will contain properties that all rows and columns, regardless of size, will possess.

```css
.row, .column {

}

.row {
  /* Let's add a border so we can see our rows better */
  border: 2px solid rebeccapurple;
}

.column {
  /* Let's add another border so we can see our columns better */
  border: 2px solid tomato;
}
```

Before we start defining widths and giving our grid system some versatility, we need to take care of a few things...

#### Box-sizing

**Q:** By default, what is the `width` property defined as?
* width = content

We want to be able to explicitly define our column widths so that they also include `border` and `padding`.
* `width` = `content` + `padding` + `border`
* This way, we know exactly how wide our columns will be.
* We can do this by changing the `box-sizing` property of our `.row` and `.column` selectors.

```css
/* styles.css */

.row, .column {
  /* By default, box-sizing is set to content-box */
  box-sixing: border-box;
}
```

#### Clearfix

**Q:** Did anybody use clearfix in their projects?

Our grid relies on being able to float columns. These columns will most likely contain content of various sizes.
* We need to make sure each piece of content is constraind to its respective row and column containers
* This is where the clearfix technique comes in and, fortunately, it's easy to implement (as long as you don't care about how your site looks in Internet Explorer).

**TODO:** Provide an example of content overflow. Codepen?
* **Why** does it happen?

```css
.row {
  overflow: auto;
}
```

Easy, right? But like I said, if we want to help out our IE friends, implementing clearfix requires a few more steps.

```css
/* The below code is in place of the previous clearfix example */

.row:before, .row:after {
  content: "";
  display: table;
}

.row:after {
  clear: both;
}
```

**TO DO:** Why does the above example work?

#### Define column behavior

So our rows are actually good to go!
* They're just horizontal containers.
* Thanks to clearfix we don't need to worry about content overflow.
* Our columns will handle page width. Let's work on that now...

```css
.column {
  float: left;
  position: relative;
}
```

**TO DO:** Explain why we `float` and `position`

**YOU DO:** Let's give our rows and columns a spin.
* Take a minute or two to create some rows and columns in `index.html` using the class selectors we just made.
* What functionality do we currently have? What do we need to add?

```html
<body>
  <div class="row">
    <div class="column">Something</div>
    <div class="column">Something</div>
    <div class="column">Something</div>
  </div>
  <div class="row">
    <div class="column">Something</div>
    <div class="column">Something</div>
    <div class="column">Something</div>
  </div>
</body>
```

Right now, we can...
* **Separate content into rows and columns.** What does it look like when we turn `float` off? How about `overflow`?

We need to...
* **Set column widths.** We don't necessarily want our column widths to be defined by their content.
* **Define total width.** In any scenario, we want our total grid width to cover the entire page.
* **Give everything some space.** Our grid will look better if we give our rows and columns some breathing room (i.e., `margin`)

#### Create columns with specific widths

**REF:** Show either example website or student whiteboard response from earlier example.

So we want to define our column widths not by the width of their content but how much of the page we want them to take up.
* e.g., a sidebar nav that takes up 1/6 of total page width.

Most grids have a column size of 12.
* That means the total column width for each row should equal 12.
* We're going to create a class selector for each column size.
  * `.column-1`: occupies 1/12 of total page width
  * `.column-3`: occupies a quarter (3/12) of total page width
  * `.column-12`: occupies entire (12/12) page width

**Q:** How are we going to define the widths for each of these classes?
* What unit should we use?
* How are we going to calculate the value?

```css
/* style.css */

.column-1 { width: 8.333%; }
.column-2 { width: 16.66%; }
.column-3 { width: 25%; }
.column-4 { width: 33.33%; }
.column-5 { width: 41.66%; }
.column-6 { width: 50%; }
.column-7 { width: 58.33%; }
.column-8 { width: 66.66%; }
.column-9 { width: 75%; }
.column-10 { width: 83.33%; }
.column-11 { width: 91.66%; }
.column-12 { width: 100%; }
```

**TO DO:** Include other types of column syntax (e.g., col-2-3 = "two thirds")

You don't have to use the same class selector syntax as the above example.
* You can and should customize your grid to fit your own needs.
* Ex. `.col-2-3` = a column that takes up 2/3 width of its parent container

Let's apply these selectors to `index.html` in a way that resembles an actual website.
* Note the addition of the `.header` `.middle` and `.footer` class selectors to our rows.

```html
<!-- index.html -->

<body>
  <div class="row header">
    <div class="column column-2">Logo</div>
    <div class="column column-4">-</div>
    <div class="column column-6">
      <ul>
        <li>Home</li>
        <li>About</li>
        <li>Contact</li>
        <li>FAQ</li>
      </ul>
    </div>
  </div>
  <div class="row middle">
    <div class="column column-2">-</div>
    <div class="column column-8">So much content.</div>
    <div class="column column-2">-</div>
  </div>
  <div class="row footer">
    <div class="column column-12">(c) Maseda Industries, 2015.</div>
  </div>
</body>
```

Let's also add some styling that will help us visualize this better.
* Note we gave our `.header` `.middle` and `.footer` selectors some explicit heights.

```css
/* style.css */

.column {
  float: left;
  position: relative;
  border: 2px solid Tomato;
  border-radius: 20px;
  text-align: center;
}

/* Let's give the sections of our site some width/height */
.header .column,
.footer .column {
  height: 50px;
  line-height: 50px;
}

.middle .column {
  height: 400px;
  line-height: 400px;
}
```

Let's take another look at our `index.html` in the browser.
* You can see our website has some form now.
* Our sections could use some space though...

#### Gutters

**Q:** How should we go about putting space between the sections of our site?
* What CSS properties do we have at our disposal?
* `margin` vs. `padding`: which one and why?

Remember when we set `box-sizing: border-box` for our columns?
* That made it so our width includes `padding`.
* We can go ahead and add `padding` to our columns without having to adjust their `width`.
  * If we added gutters using `margin`, we'd have to do some math.

///////////////
- finish gutters
  - figure out padding vs. margin
- add exercise where students create their own grid structure
  - ask them to match a particular website/format
- front-end frameworks
///////////////

## Exercise: Build your own grid from scratch

Use what we have learned in-class so far to build a grid from scratch.
* Feel free to use the same class selector syntax. Or create your own!
* **TO DO:** Match a real-world example?

## Front-End Frameworks

**Q:** Did anybody use a CSS front-end framework for their project?

**Q:** What is a CSS front-end framework?
* Like a library, that it gives us a toolkit that we can use to streamline the front-end development process.
* But it's a framework, so that means we need to follow a certain structure.
  * **TODO:** What is that structure? Why is it not a library?

There are tons of front-end frameworks out there -- like Bootstrap, Foundation, Material Design -- that incorporate grid systems.
* They're not necessarily better. In fact, if you're only looking to implement a grid system and not any additional styling, you might be better off building a grid from scratch.
* Nevertheless, you will encounter these frameworks in the wild so let's get some experience with them.

**EXERCISE:** Let's implement the grid you created in the last exercise using Bootstrap.
  1. Link the Bootstrap stylesheet to your `index.html` file [using a CDN](http://getbootstrap.com/getting-started/).
  2. Implement the same grid from the last exercise using Bootstrap row and column class selectors.
  3. If you finish early, try adding some styling to your blog.

## Additional Reading

* [Learn Layout: Clearfix](http://learnlayout.com/clearfix.html)
