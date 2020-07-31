<script>
  import { onMount, afterUpdate } from "svelte";
  import pdfjs from "pdfjs-dist";
  import Lazy from "./Lazy.svelte";
  import Page from "./Page.svelte";

  //URL of the Document to Display
  export let url = "assets/test.pdf";
  export let scale = 1;

  let documentProxy = null;
  let pageCount = 0;
  let pages = [];

  $: onMount(() => {
    documentProxy = pdfjs.getDocument(url);
    documentProxy.promise.then(function(D) {
      pageCount = D.numPages;
    });
  });

  $: afterUpdate(() => (pages = Array.from(Array(pageCount), (_, i) => i + 1)));
</script>

<style>
  #pdf-container {
    margin: 1vh;
    position: fixed;
    top: 0px;
    left: 0px;
    height: 98vh;
    width: 40vw;
    overflow: scroll;
    border-radius: 0.5vh;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.07), 0 2px 4px rgba(0, 0, 0, 0.07),
      0 4px 8px rgba(0, 0, 0, 0.07), 0 8px 16px rgba(0, 0, 0, 0.07),
      0 16px 32px rgba(0, 0, 0, 0.07), 0 32px 64px rgba(0, 0, 0, 0.07);
  }

  /* Hide scrollbar for Chrome, Safari and Opera */
  .example::-webkit-scrollbar {
    display: none;
  }

  /* Hide scrollbar for IE, Edge and Firefox */
  .example {
    -ms-overflow-style: none; /* IE and Edge */
    scrollbar-width: none; /* Firefox */
  }
</style>

<div id="pdf-container" class="example">

  {#each pages as pageNum}
    <Lazy offset={10} fadeOption={{ delay: 500, duration: 1000 }}>
      <Page {documentProxy} {pageNum} {scale} />
    </Lazy>
  {/each}
</div>
