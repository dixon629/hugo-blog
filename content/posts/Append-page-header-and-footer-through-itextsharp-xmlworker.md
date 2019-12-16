+++
title = "Append page header and footer through itextsharp xmlworker"
date =  2015-04-18T13:07:42+08:00
draft = true
tags = ["itextsharp","PDF"]
categories = ["c#"]
+++
We’re using itextSharp xmlworker to convert html to PDF.  And we need to append page header and footer for PDF, but xmlworker doesn’t support this. So how to do it? Actually xmlworker still use itextsharp to generate PDF, thus we’re able to use page event to do it as well.

We want to specify the header and footer as html. So we need to parse html to itext elements first. XMLWorkerHelper only supports to parse html to elements with **css string**, so I add this below method to support **css path**.

```c#
/// <summary>
/// Parse html to PDF element list by html content and css path
/// </summary>
/// <param name="html">html</param>
/// <param name="cssPath">css full path</param>
/// <returns>PDF element list</returns>
public static ElementList ParseToElementListByCssPath(string html, string cssPath)
{
    ICSSResolver cssResolver = new StyleAttrCSSResolver();
    if (cssPath != null)
        cssResolver.AddCssFile(cssPath, true);

    //HTML
    HtmlPipelineContext htmlContext = new HtmlPipelineContext(null);
    htmlContext.SetTagFactory(Tags.GetHtmlTagProcessorFactory());
    htmlContext.AutoBookmark(false);

    // Pipelines
    ElementList elements = new ElementList();
    ElementHandlerPipeline end = new ElementHandlerPipeline(elements, null);
    HtmlPipeline htmlPipeline = new HtmlPipeline(htmlContext, end);
    CssResolverPipeline cssPipeline = new CssResolverPipeline(cssResolver, htmlPipeline);

    // XML Worker
    XMLWorker worker = new XMLWorker(cssPipeline, true);
    XMLParser parser = new XMLParser(worker);
    using (var reader = new StringReader(html))
    {
        parser.Parse(reader);
    }

    return elements;
}
```

Once we parse the html to the itext elements, we can process it in PDF page event. And then we need to set the page event when xmlworker parse html.
This is a demo how to set page event.

```c#
/// <summary>
/// Convert html to PDF
/// </summary>
/// <param name="html">html content</param>
/// <param name="pageEvent">PDF page event</param>
/// <param name="cssPath">css full path</param>
/// <returns>PDF binary data</returns>
public byte[] convert(string html, string cssPath, IPdfPageEvent pageEvent)
{
    using (Document document = new Document())
    {
        using (var outStream = new MemoryStream())
        {
            PdfWriter writer = PdfWriter.GetInstance(document, outStream);
            // page event
            if (pageEvent != null)
                writer.PageEvent = pageEvent;

            writer.InitialLeading = 12.5f;
            document.Open();

            // css
            ICSSResolver cssResolver = XMLWorkerHelper.GetInstance().GetDefaultCssResolver(false);
            if (cssPath != null)
                cssResolver.AddCssFile(cssPath, true);

            // html 
            var htmlContext = new HtmlPipelineContext(null);
            htmlContext.SetTagFactory(Tags.GetHtmlTagProcessorFactory());

            // pipelines
            PdfWriterPipeline pdfPipeline = new PdfWriterPipeline(document, writer);
            HtmlPipeline htmlPipeline = new HtmlPipeline(htmlContext, pdfPipeline);
            CssResolverPipeline cssPipeline = new CssResolverPipeline(cssResolver, htmlPipeline);

            // parse
            XMLWorker worker = new XMLWorker(cssPipeline, true);
            XMLParser parser = new XMLParser(worker);
            using (var reader = new StringReader(html))
            {
                parser.Parse(reader);
            }

            document.Close();
            return outStream.ToArray();
        }
    }
}
```
