# Frontend Mentor - Blog preview card solution

This is a solution to the [Blog preview card challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/blog-preview-card-ckPaj01IcS). Frontend Mentor challenges help you improve your coding skills by building realistic projects.

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued development](#continued-development)
  - [Useful resources](#useful-resources)
- [Author](#author)

## Overview

### The challenge

Users should be able to:

- See hover and focus states for all interactive elements on the page

### Screenshot

|  Mobile designed at 375px:   |  Desktop designed at 1440px:  |
| :--------------------------: | :---------------------------: |
| ![](./screenshot-mobile.png) | ![](./screenshot-desktop.png) |

### Links

- Solution URL: [https://github.com/elisilk/blog-preview-card-main](https://github.com/elisilk/blog-preview-card-main)
- Live Site URL: [https://elisilk.github.io/blog-preview-card-main/](https://elisilk.github.io/blog-preview-card-main/)

## My process

### Built with

- Semantic HTML5 markup
- CSS custom properties
- Flexbox
- Mobile-first workflow

### What I learned

#### Fluid Typography

I took on the challenge of: "The font sizes in this project are slightly smaller in the mobile layout. Find a way to reduce font size for smaller screens without using media queries." So I will share my solution to that and the thinking that went into it.

##### Resources I Used

- [Frontend Mentor - Adapting typography across devices](https://www.frontendmentor.io/learning-paths/building-responsive-layouts--z1qCXVqkD/steps/669710ac85c9917334f60eb0/article/read)
- [Modern Fluid Typography Using CSS Clamp](https://www.smashingmagazine.com/2022/01/modern-fluid-typography-css-clamp/)
- [Responsive And Fluid Typography With vh And vw Units](https://www.smashingmagazine.com/2016/05/fluid-typography/)
- [Modern Fluid Typography with clamp()](https://chriskirknielsen.com/blog/modern-fluid-typography-with-clamp/)
- [web.dev - Typography](https://web.dev/learn/design/typography)
- [Relative length units](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units)
- [clamp](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp)

An amazingly-implemented tool for fluid typography calculations:

- [Easy Fluid Typography](https://ryanfeigenbaum.com/fluid-typography/)
- [The Clamp Calculator](https://royalfig.github.io/fluid-typography-calculator/)
- [But, of course, there are so many more...](https://www.google.com/search?q=css+fluid+typography+calculator)
  - [Modern fluid typography editor](https://modern-fluid-typography.vercel.app/)
  - [Fluid Type Scale Calculator](https://www.fluid-type-scale.com/)

##### The General Advantages

After reading a bunch of these articles, I felt I understood the general idea: Use the built-in [`clamp`](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp), [`calc`](https://developer.mozilla.org/en-US/docs/Web/CSS/calc), and relative units (`em` + `vw`) to both set the lower and upper bounds of the font size, while still allowing the size to adjust automatically to the screen size within those bounds.

I totally see how that is a powerful solution that eliminates the need to use multiple media queries at each breakpoint, which then means "Switching between fixed text sizes at specific breakpoints is quite jumpy" [https://web.dev/learn/design/typography](citation).

##### The General Form of the Idea

So here is the general form of the clamp function `clamp(min, val, max)`, and here is the typical font size example given:

```css
font-size: clamp(1rem, 0.8rem + 1vw, 3rem);
```

Okay, so choosing the upper and lower bounds seems fairly straightforward to me: choose the font-size in the mobile design for the lower bound and the font-size in the desktop design for the upper bound.

##### Design Font Sizes

In this design, the font sizes for the various page elements were:

| ID          | Mobile          | Desktop         |
| ----------- | --------------- | --------------- |
| Category    | 12px = 0.75rem  | 14px = 0.875rem |
| Date        | 12px = 0.75rem  | 14px = 0.875rem |
| Title       | 20px = 1.25rem  | 24px = 1.5rem   |
| Description | 14px = 0.875rem | 16px = 1rem     |
| Author      | 14px = 0.875rem | 14px = 0.875rem |

But then how the heck do you decide what values to put into the middle preferred value area? Looks to me like some kind of baseline `rem` value + some fluid `vw` component. 🤔

##### Vary Between what Screen Sizes?

I started asking myself what are the screen widths that I want the font size to vary between? What is the minimum screen width that I am comfortable with the mobile font size? And what is the maximum screen width that I am comfortable with the desktop font size? Clearly, I want these to be greater than the mobile design width (320px) and less than the desktop design width (1440px). So let's just pick something nice and round, say 400px (min) to 1200px (max). Those numbers are round themselves, but the span/range between them is also nicely divisible by 16 (because 16px is the base `rem` value), so I believe that will make our `vw` values work out nicer in the end.

- Min screen size: 500px = 31.25rem
- Max screen size: 1000px = 62.5rem
- Span/range: 1000px - 500px = 500px = 31.25rem

##### The Fluid Component?

Okay, so what is an appropriate (a) baseline and (b) fluid component that will vary between those two?

[`vw`](https://developer.mozilla.org/en-US/docs/Web/CSS/length#vw) represents a percentage and so we need to multiply by 100. So to get how much `vw` units we have available to us at the starting screen width, we do the following calculation:

- 100 x 2px in font size / 500px in screen width = 0.4vw

So that tells us that `0.4vw` will add `2px` to our font size at the small end of our range. For the larger end of our range, we want to add on `4px`, so let's calculate that:

- 100 x 4px in font size / 0.4vw = 1000px in screen size

Now let's also pick a round baseline value that is also easily divisibly by 16. This value should be smaller than our target font sizes, as we will add on a fluid component to it:

- Baseline: .5rem = 8px

So that means I want the fluid component to vary between 6px and 8px to result in a final preferred font size value of 14px - 16px. So what `vw` value at the min and max screen sizes gets me to that min and max preferred font size?

[`vw`](https://developer.mozilla.org/en-US/docs/Web/CSS/length#vw) represents a percentage and so we need to multiply by 100.

- 100 x 2px in font size / 500px in screen width = 0.4vw
- 100 x 4px in font size / 0.4vw = 1000px in screen size

```
-- ** desired clamp btwn 14px - 16px ** ---
font-size: clamp(0.875rem, 0.75rem + 0.4vw, 1rem);
.75rem = 12px
reaches min at - 500px -- 12px + 2px = 14px
reaches max at - 1000px -- 12px + 4px = 16px
```

- 2px in font size / 400px in screen width = 0.005vw
- 4px in font size / 0.005vw = 800px in screen size

- 8px in font size / 400px in screen width = 0.02vw
- 10px in font size / 0.02vw = 500px in screen size

- 6px in font size / 400px in screen width = 0.015vw
- 8px in font size / 0.015vw = 533px in screen size

```
-- ** desired clamp btwn 14px - 16px ** ---
clamp(0.875rem, 0.5rem + 0.015vw, 1rem);
.5rem = 8px
reaches min at - 400px -- 8px + 6px = 14px
reaches max at - 1200px -- 8px + 8px = 16px
```

```
-- ** desired clamp btwn 14px - 16px ** ---
clamp(0.875rem, 0.625rem + 1vw, 1rem);
.625rem = 10px
reaches min at - 400px -- 10px + 4px = 14px
reaches max at - 600px -- 10px + 6px = 16px
```

#### Trying Another Route to Understanding

[Modern Fluid Typography with clamp()](https://chriskirknielsen.com/blog/modern-fluid-typography-with-clamp/)

$max-vw = 62.5rem = 1000px
$min-vw = 31.25rem = 500px

$max-value = 16px = 1rem
$min-value = 14px = 0.875rem

62.5 - 31.25 = 31.25rem -- span of vw for preferred values

1 - 0.875 = 0.125rem -- span of rem for preferred values

1 / 31.25 = 0.032 --

0.032 x 0.125 = 0.004 = $factor

0.125 / 31.25 = 0.004 = $factor

.875 - (31.25 x 0.004) = 0.75rem

100 x 0.004 = .4vw

$factor: 1 / ($max-vw - $min-vw) * ($max-value - $min-value);

$calc-value: unquote('#{ $min-value - ($min-vw _ $factor) } + #{ 100vw _ $factor }');

#### Other Ideas I Worked Through

Unrelated to the fluid typography, I also had to look up a few other small things related to the box shadow and to the sizing of the illustration image:

- [Box shadows](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow) - I had to look up the shorthand for this. The box shadow for this design was pretty easy given that there was no blurring at all. And it was also fun to play with the [Box-shadow generator](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_backgrounds_and_borders/Box-shadow_generator).
- [How to fill a box with an image without distorting it](https://developer.mozilla.org/en-US/docs/Learn/CSS/Howto/Fill_a_box_with_an_image) - Used `object-fit: cover` and `height: 200px` to make sure the image had the appropriate height for the design, filled the space completely, and was not distorted.

### Continued development

Working on this.

### Useful resources

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web) - Of course, as always. So useful.

## Author

- Website - [Eli Silk](https://github.com/elisilk)
- Frontend Mentor - [@elisilk](https://www.frontendmentor.io/profile/elisilk)
