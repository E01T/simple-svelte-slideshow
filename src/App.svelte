<script>
  import SliderGalery from "./SliderGallery.svelte";

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