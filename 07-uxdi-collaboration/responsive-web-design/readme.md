# Responsive Web Design

## Learning Objectives

- Use media queries to adjust styles for different viewport sizes.
- Explain the difference between a responsive website and a mobile-specific website.
- Identify and use relative units like `em`, `rem`, `vh`, `vw`, etc..
- List the different media query values and their conditions.
- Use the iOS simulator, Safari console, and Chrome Dev tools to debug responsive CSS.
- Compare mobile-first to desktop-first responsive design

## Opening

Ultimately we are trying to answer the question:

>How do we build web applications and sites for an optimal interaction experience on a multitude of devices?

### How we got here

In the beginning, there was no CSS. Every site looked nearly identical. As styling web pages became more common,
developers began structuring their pages with table layouts and fixed-width CSS.

Many designers still ask

>What are the most common dimensions for a website design?

You tell me! http://screensiz.es

## Mobile Specific Sites

One way to create optimal experiences for mobile users is a dedicated mobile site

You know you're on one when you see `m.` in the url!

## Media Queries

One way to adjust the styles depending on the device's size is by using media queries:

```css
body{
  background: papayawhip;
}

@media (max-width: 400px){
  body{
    background: tomato;
  }
}
```

The `max-width` is the device's viewport size.

Other possible values include 
   
```
width | min-width | max-width
| height | min-height | max-height
| device-width | min-device-width | max-device-width
| device-height | min-device-height | max-device-height
| aspect-ratio | min-aspect-ratio | max-aspect-ratio
| device-aspect-ratio | min-device-aspect-ratio | max-device-aspect-ratio
| color | min-color | max-color
| color-index | min-color-index | max-color-index
| monochrome | min-monochrome | max-monochrome
| resolution | min-resolution | max-resolution
| scan | grid
```

## You do

Working with the example above, create a fiddle/codepen/webpage that includes at least two media queries.

e.g. when the viewport is less than 800px wide, make the background yellow. When the viewport is less
than 400px wide, make the background green.

As a bonus, try out a few of the properties above. You can combine media queries to get several different results.

i.e. what combination of media queries could produce the following grid?

```
  green   | yellow | red
 -------- | -----  | ---
turqouise | green  | purple 
```

## Mobile first vs Desktop first

What is the difference between starting with the smallest viewport and applying styles as the viewport size increases
and starting with the largest viewport size as the default?

## Relative units of measurement

So far, we've been working with pixels (absolute unit of measurement) and percentages (relative unit of measurements)

### You do: research the following units

- em and rem
- vh and vw
- vmin and vmax
- ex and ch

## We do: Convert the "Craigslist Grid"

Let's convert the grid we had yesterday to adjust the layout when the viewport size changes.

## You do: 007 Exercise
