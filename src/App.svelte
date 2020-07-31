<script>
  import PdfViewer from "./PdfViewer/PdfViewer.svelte";

  let url = "assets/test.pdf";
</script>

<style>
  .sub-container {
    position: fixed;
    margin: 30px;
    left: 50vw;
    width: 50vw;
  }
</style>

<PdfViewer {url} />

<!-- Additional Content wich can be deleted-->
<div class="sub-container">
  <h3>;-)</h3>
</div>
