# Simple Svelte Slideshow

[Demo](https://optimistic-carson-820f70.netlify.com/) page.

As the name suggests, it is a simple svelte slideshow
It uses the tweened store from "svelte/motion"

## Get started

First install the slideshow

```bash
npm install svelte-slideshow-gallery
```

Then on the top of your file you wish to use it add the following import statement

```javascript
import SliderGalery from "SliderGallery";
```

Finaly add the following component into you markup

```html
<SliderGalery {imgArray} />
```

## Options

The only required prop is `imgArray` which is an array of objects.
The objects should have two properties:

- The `src` property which holds the image source/path
- And a `text` property which holds the image description (see example below)

```javascript
// These are images in the public folder
let terre = "images/img_5terre_wide.jpg";
let lights = "images/img_lights_wide.jpg";
let mountains = "images/img_mountains_wide.jpg";
let nature = "images/img_nature_wide.jpg";
let snow = "images/img_snow_wide.jpg";
let woods = "images/img_woods_wide.jpg";
// imgArray is the name of the property
let imgArray = [
  { src: terre, text: "Cinque Terre" },
  { src: lights, text: "Northen Lights" },
  { src: mountains, text: "Mountains and fjords" },
  { src: nature, text: "Nature and sunrise" },
  { src: woods, text: "The Woods" },
  { src: snow, text: "Snowy Mountains" }
];
```

Now the imgArray should be passed as a prop into the component

```html
<SliderGalery {imgArray} />
```

And you are good to go.

There are three more optional properties:

1. delay
2. duration
3. easingMethod

`delay` is set to zero by default. The `duration` of the animations defaults to 1.5 seconds.
For the `easingMethod` you can choose between five values:

- sineInOut
- cubicInOut,
- expoInOut,
- backInOut,
- elasticInOut

  For more information about these methods visit the following [link](https://svelte.dev/examples#easing)

  Here is another example:

```javascript
<script>
  import SliderGalery from "SliderGallery";

  // These are in the public folder
  let terre = "images/img_5terre_wide.jpg";
  let lights = "images/img_lights_wide.jpg";
  let mountains = "images/img_mountains_wide.jpg";
  let nature = "images/img_nature_wide.jpg";
  let snow = "images/img_snow_wide.jpg";
  let woods = "images/img_woods_wide.jpg";
  let imgArray = [
    { src: terre, text: "Cinque Terre" },
    { src: lights, text: "Northen Lights" },
    { src: mountains, text: "Mountains and fjords" },
    { src: nature, text: "Nature and sunrise" },
    { src: woods, text: "The Woods" },
    { src: snow, text: "Snowy Mountains" }
  ];
  const easingMethod = "backInOut";
  const duration = 2000;
</script>

<main>
	<SliderGalery {imgArray} {duration} {easingMethod}/>
</main>
```

Happy coding...
