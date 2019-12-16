+++
title = "How to generate identical chart in html and PDF"
date =  2015-04-15T22:18:20+08:00
draft = true
tags = ["Phantomjs"]
categories = ["c#"]
+++

I’m working on PDF stuff recently. I got a requirement that the chart in PDF should be identical with in html. 

At the beginning, we were trying to generate chart of PDF through .NET  and make same styles with html. But we found it’s very hard to make them identical as they used different technology, d3js for client-side, Microsoft charts for server-side.

Then I found out Phantomjs. We planned to generate whole PDF by Phantomjs. But we got the other issue, there were some table in the PDF, and we needed to repeat the table header when table crossing pages, but Phantomjs doesn’t support this feature. Finally we decided to use Phantomjs to convert chart to image and then generate PDF through itextSharp.

*This is my codes to call Phantomjs on .NET to covert html to image.*
{{< gist dixon629 548cb3ddb842fc3fc48d >}}