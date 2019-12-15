+++
title = "How to export nvd3 chart to PNG"
date =  2015-03-28T16:41:23+08:00
draft = true
tags = ["javascript"]
categories = ["d3","nvd3","chart","canvg","angularjs"]
+++

Recently I was working on NVD3. I got a task to export [nvd3](https://github.com/novus/nvd3) chart to PNG file.
It needs to support IE9+, Chrome, Firefox. I encountered some problems when I was doing this task, it took me some time to figure it out. I think it is better to share my experience for who has similar requirements.

*nvd3* is base on [d3js](http://d3js.org/). *d3js* uses SVG to render data. So the key issue is how to covert SVG to PNG, and the other issue is how to download the PNG file.

## Solution 1: send SVG  to Server , convert it to PNG and generate link to download
Our server-side is Asp.net, so I did some research how to convert SVG to PNG on .net. I got three options,[SVG.NET](https://github.com/vvvv/SVG),[ ImageMagick.NET](http://imagemagick.codeplex.com/),[inkscape](https://inkscape.org/).    

I tried them out, but no one can generate the PNG perfectly. The *nvd3* uses some CSS class for SVG, but *SVG.NET* and *mageMagick.NET* don't support it very well. And the PNG file that *inkscape* generates are  different with browsers render. I tried to add the styles to SVG file manually without CSS, but PNG file they generated still looks different with browsers render. 

Finally I found [PhantomJs](http://phantomjs.org/) out. *PhantomJS* is a headless WebKit scriptable with a JavaScript API. *PhantomJS* is a perfect solution to render SVG and can export to image and PDF. It can generate PNG exactly same with browsers render. But I have to install it on the server and write some API to call it. 
**I discussed with my leader, but she didn't like this solution, so I have to turn to Solution 2.**
## Solution 2: convert SVG to html5 canvas , then convert canvas to PNG and download it
### Step 1: convert SVG to canvas
[saveSvgAsPng](https://github.com/exupero/saveSvgAsPng) is a library to export SVG to PNG on the client-side. But unfortunately, it doesn't work on IE and safari. There is a security error on IE. And besides,It can't download the PNG file on safari, safari open the file on the new tab instead of downloading it. So I have to give up this scenario.

And then I found [canvg](https://github.com/gabelerner/canvg) out, *canvg* can parse SVG and render it on Canvas. But *canvg *doesn't support external CSS on the SVG. 

Eventually I was inspired by the *saveSvgAsPng*. *saveSvgAsPng* will read the used external CSS for SVG and put it into the SVG. So I create a new function in *saveSvgAsPng* called **saveAsSVGWithStyle**.It will return a new SVG with CSS.

{{< highlight javascript >}}
out$.saveAsSVGWithStyle = function (el, options, cb) {
      options = options || {};
      options.scale = options.scale || 1;
      var xmlns = "http://www.w3.org/2000/xmlns/";

      inlineImages(el, function() {
          var outer = document.createElement("div");
          var clone = el.cloneNode(true);
          var width, height;
          if(el.tagName == 'svg') {
              width = parseInt(clone.getAttribute('width') || clone.style.width || out$.getComputedStyle(el).getPropertyValue('width'));
              height = parseInt(clone.getAttribute('height') || clone.style.height || out$.getComputedStyle(el).getPropertyValue('height'));
          } else {
              var box = el.getBBox();
              width = box.x + box.width;
              height = box.y + box.height;
              clone.setAttribute('transform', clone.getAttribute('transform').replace(/translate\(.*?\)/, ''));

              var svg = document.createElementNS('http://www.w3.org/2000/svg','svg')
              svg.appendChild(clone)
              clone = svg;
          }

          clone.setAttribute("version", "1.1");
          clone.setAttributeNS(xmlns, "xmlns", "http://www.w3.org/2000/svg");
          clone.setAttributeNS(xmlns, "xmlns:xlink", "http://www.w3.org/1999/xlink");
          clone.setAttribute("width", width * options.scale);
          clone.setAttribute("height", height * options.scale);
          clone.setAttribute("viewBox", "0 0 " + width + " " + height);
          outer.appendChild(clone);

          var css = styles(el, options.selectorRemap);
          var s = document.createElement('style');
          s.setAttribute('type', 'text/css');
          s.innerHTML = "<![CDATA[\n" + css + "\n]]>";
          var defs = document.createElement('defs');
          defs.appendChild(s);
          clone.insertBefore(defs, clone.firstChild);

          var svg = doctype + outer.innerHTML;
          if (cb) {
              cb(svg, width,height);
          }
      });
  }
{{< /highlight >}}

Then we can convert SVG with CSS to canvas through *canvg*.
{{< highlight javascript >}}
              saveAsSVGWithStyle(svgElement.node(), {}, function (svgWithStyle, width, height) {
                    var canvas = document.createElement('canvas');
                    canvas.width = scale * width;
                    canvas.height = scale * height;
                    canvg(canvas, svgWithStyle, { scaleWidth: canvas.width, scaleHeight: canvas.height, ignoreDimensions: true, ignoreMouse: true, ignoreClear: true });
                });
{{< /highlight >}}

When I tested on different browsers. I found that IE9 doesn't support  SVG **defs**. So I can't just define *defs and put CSS in defs*. I have to add style property to the SVG manually like this. 
**If you have a better solution to make external CSS into the DOM style property, let me know, thank you.**
{{< highlight javascript >}}
            var _appendStylesToSVG = function (svgElement) {
                svgElement.selectAll('.nvd3 .nv-axis').style('pointer-events', 'none');
                svgElement.selectAll('.nvd3 .nv-axis path').style('fill', 'none');
                svgElement.selectAll('.nvd3 .nv-axis path').style('stroke', 'rgb(204, 204, 204)');
                svgElement.selectAll('.nvd3 .nv-axis path').style('stroke-opacity', '0.75');
                svgElement.selectAll('.nvd3 .nv-axis path').style('shape-rendering', 'crispedges');
                svgElement.selectAll('.nvd3 .nv-axis path.domain').style('stroke-opacity', '0.75');
                svgElement.selectAll('.nvd3 .nv-axis.nv-x path.domain').style('stroke-opacity', '0');
                svgElement.selectAll('.nvd3 .nv-axis line').style('fill', 'none');
                svgElement.selectAll('.nvd3 .nv-axis line').style('stroke', 'rgb(229, 229, 229)');
                svgElement.selectAll('.nvd3 .nv-axis line').style('shape-rendering', 'crispedges');
                svgElement.selectAll('.nvd3 .nv-axis .zero line, .nvd3 .nv-axis line.zero').style('stroke-opacity', '0.75');
                svgElement.selectAll('.nvd3 .nv-multibar .nv-groups rect').style('stroke-opacity', '0');
                svgElement.selectAll('.nvd3 .nv-multibar .nv-groups rect').style('transition', 'fill-opacity 250ms linear 0s');
                svgElement.selectAll('.nvd3 .nv-multibarHorizontal .nv-groups rect').style('stroke-opacity', '0');
                svgElement.selectAll('.nvd3 .nv-multibarHorizontal .nv-groups rect').style('transition', 'fill-opacity 250ms linear 0s');
                svgElement.selectAll('.nvd3 .nv-discretebar .nv-groups rect').style('stroke-opacity', '0');
                svgElement.selectAll('.nvd3 .nv-discretebar .nv-groups rect').style('transition', 'fill-opacity 250ms linear 0s');
                svgElement.selectAll('.nv-x .tick line').style('display', 'none');
                svgElement.selectAll('.statistics-title').style('fill', 'rgba(107, 156, 188, 1)');
                svgElement.selectAll('.statistics-title').style('font-size', '21px');
                svgElement.selectAll('.statistics-title').style('font-weight', '300');
                svgElement.selectAll('.statistics-title-total').style('font-size', '14px');
                svgElement.selectAll('.statistics-title-total').style('fill', 'rgb(153, 153, 153)');
                svgElement.selectAll('.statistics-title-total').style('font-weight', '300');
            };
{{< /highlight >}}
### Step 2: downloading PNG
We have to support IE9, but IE9 doesn't support html5 file API. So I did a trik, I sent  base64 string of the image to server-side, server-side would generate a link to client-side, and then download it. The completed codes is
{{< highlight javascript >}}
            $scope.exportToPNG = function (chartId,scale) {
                var svgElement = d3.select("#" + chartId + " svg");
                //Append styles as there are some problems when drawing PNG on IE 9
                _appendStylesToSVG(svgElement);
                var fileName = _buildPNGName(chartId);
                scale = scale ? scale : 1.5;

                saveAsSVGWithStyle(svgElement.node(), {}, function (svgWithStyle, width, height) {
                        var canvas = document.createElement('canvas');
                    canvas.width = scale * width;
                        canvas.height = scale * height;
                        canvg(canvas, svgWithStyle, { scaleWidth: canvas.width, scaleHeight: canvas.height, ignoreDimensions: true, ignoreMouse: true, ignoreClear: true });
                        var imageData = canvas.toDataURL("image/png");
                        imageData = imageData.replace(/^data:image\/(png|jpg);base64,/, "");
                        statisticalChartsService.saveChartCache(imageData).then(function (response) {
                            statisticalChartsService.getChartCache(fileName, response.data);
                        });
                    });
            };
{{< /highlight >}}
## Conclusion
If you'd like to export SVG to PNG on the server-side, **PhantomJs** is a good choise. If you'd like to SVG to PNG on the client-side, you can use **canvg**.  But for **nvd3**, you have to figure out how to add external CSS class to SVG.