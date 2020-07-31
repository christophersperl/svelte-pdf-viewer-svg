<script>
  import pdfjs from "pdfjs-dist";
  import pdfjsWorker from "pdfjs-dist/build/pdf.worker.entry";

  export let pageNum = 1;
  export let scale = 1.5;
  export let documentProxy = null;
  export let rotation = 0;

  pdfjs.GlobalWorkerOptions.workerSrc = pdfjsWorker;

  documentProxy.promise.then(function(pdfDoc) {
    pdfDoc.getPage(pageNum).then(function(page) {
      let viewport = page.getViewport({ scale: scale, rotation: rotation });
      let container = document.getElementById("the-svg");
      container.style.width = viewport.width + "px";
      container.style.height = viewport.height + "px";

      page
        .getOperatorList()
        .then(function(opList) {
          var svgGfx = new pdfjs.SVGGraphics(page.commonObjs, page.objs);
          return svgGfx.getSVG(opList, viewport);
        })
        .then(function(svg) {
          container.appendChild(svg);
        });
    });
  });
</script>

<div id="the-svg" />
