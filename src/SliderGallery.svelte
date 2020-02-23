<script>
  import { tweened } from "svelte/motion";
  import {
    sineInOut,
    cubicInOut,
    expoInOut,
    backInOut,
    elasticInOut
  } from "svelte/easing";
  import { onMount, beforeUpdate } from "svelte";

  const easing = {
    sineInOut,
    cubicInOut,
    expoInOut,
    backInOut,
    elasticInOut
  };

  export let imgArray = [];
  export let delay = 0;
  export let duration = 1500;
  export let easingMethod = sineInOut;

  const imgArrayLength = imgArray.length;
  let currentIndex = 0;
  let currentImage = imgArray[currentIndex] || [];

  const opacity = tweened(1, {
    delay,
    duration,
    easing: easing[easingMethod]
  });

  const getThumbnails = fn => {
    const thubmnails = Array.from(
      document.getElementsByClassName("thumbnails-img")
    );
    thubmnails.length && fn(thubmnails);
  };

  const styleThumbnail = array => {
    array.forEach(arrayEl => arrayEl.removeAttribute("style"));
    // cssText sets multiple styles in a single statement
    array[getCurrentIndex()].style.cssText =
      "scale: 1.1; opacity: 1; z-index: 12";
  };

  onMount(() => {
    getThumbnails(styleThumbnail);
  });

  beforeUpdate(() => {
    getThumbnails(styleThumbnail);
  });

  const isArrowLeft = key => key === "ArrowLeft";
  const isArrowRight = key => key === "ArrowRight";
  const isImgDataset = imgData => imgData !== undefined;
  const getCurrentIndex = () => currentIndex;
  const calculateIndex = a => b => a - Math.floor(a / b) * b;

  const setCurrentIndex = newIndex => (currentIndex = newIndex); // data mutation
  const setCurrentImage = index => (currentImage = imgArray[index]); // data mutation
  const arrowIndex = num => () => getCurrentIndex() + num;
  const datasetIndex = num => () => num; // identity fn
  const exit = () => {
    return false;
  };
  const selectImage = e => {
    let i;
    // there is a catch in the "isArrowLeft(e.key) && arrowIndex(-1)" expression.
    // if we substitute with (getCurrentIndex() - 1) works for all cases except
    // when index equals to 0; in this case we get false so it evaluates the
    // rest of the expressions. To avoid that we use closures OR the commented code below.
    i =
      (isArrowLeft(e.key) && arrowIndex(-1)) ||
      (isArrowRight(e.key) && arrowIndex(1)) ||
      (isImgDataset(e.target.dataset.img) &&
        datasetIndex(+e.target.dataset.img));

    if (i === false) return false;
    i = i();
    /*
    if (isArrowLeft(e.key)) {
      i = e.key === "ArrowLeft" && getCurrentIndex() - 1;
    } else if (isArrowRight(e.key)) {
      i = e.key === "ArrowRight" && getCurrentIndex() + 1;
    } else if (e.target.dataset.img !== undefined) {
      i = +e.target.dataset.img;
    } else return;
    */
    opacity.set(0.3, { duration: 10 });
    setCurrentImage(setCurrentIndex(calculateIndex(i)(imgArrayLength))); // mutation
    opacity.update(n => n + 0.7, { delay: 100 });
  };
  const getIndex = index => index;
</script>
<!-- HTML *********************************************************************** HTML -->
<svelte:window on:keydown="{selectImage}" />
{#if imgArrayLength}
<div id="gallery">
  <figure>
    <img
      style="opacity:{$opacity}"
      id="main-image"
      src="{currentImage.src}"
      alt="{currentImage.text}"
    />
    <a on:click="{selectImage}" data-img="{currentIndex-1}" id="previous"
      >&#10094;</a
    >
    <a on:click="{selectImage}" data-img="{currentIndex+1}" id="next"
      >&#10095;</a
    >
    <figcaption>{currentImage.text || ""}</figcaption>
  </figure>
  <div on:click="{selectImage}" id="thumbnails">
    {#each imgArray as image, i}
    <img
      class="thumbnails-img"
      src="{image.src}"
      alt="{image.text}"
      data-img="{i}"
    />
    {/each}
  </div>
</div>
{/if}
<!-- CSS ************************************************************************* CSS -->
<style>
  #gallery {
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    max-width: 800px;
    margin: 0 auto;
  }

  figure {
    margin: 0;
    position: relative;
  }

  #main-image {
    width: 100%;
  }

  a {
    cursor: pointer;
    position: absolute;
    top: 40%;
    width: auto;
    padding: 16px;
    margin-top: -50px;
    color: white;
    font-weight: bold;
    font-size: 20px;
    border-radius: 0 3px 3px 0;
    user-select: none;
    -webkit-user-select: none;
    text-decoration: none;
  }

  a#previous {
    left: 0px;
  }

  a#next {
    right: 0;
  }

  a:hover {
    background-color: rgba(0, 0, 0, 0.8);
    outline-width: 0;
  }

  figcaption {
    height: 1.1rem;
    text-align: center;
    background-color: black;
    color: #fffbc4;
    font-size: 1em;
    font-family: "Times New Roman", Times, serif;
    padding: 1em;
    margin-top: -4px;
    text-shadow: 10px 1px 10px yellow, -5px 1px 10px yellow;
    letter-spacing: 2px;
  }

  #thumbnails {
    display: flex;
    flex-wrap: wrap;
  }

  .thumbnails-img {
    box-shadow: 5px 5px 4px grey;
    flex: 0 0 25%;
    width: 25%;
    /* height: 25%; */
    opacity: 0.8;
  }

  .thumbnails-img:hover {
    scale: 1.1;
  }

  @media screen and (max-width: 450px) {
    #gallery {
      width: 100%;
      padding: 0px;
      margin: 0px;
    }

    figcaption {
      padding: 0.5em;
      font-size: 0.9em;
    }
  }
</style>
