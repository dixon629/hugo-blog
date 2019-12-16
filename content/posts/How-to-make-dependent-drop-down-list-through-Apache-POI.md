+++
title = "How to make dependent drop-down list in Excel through Apache POI"
date =  2015-04-25T14:45:56+08:00
draft = true
tags = ["Apache","POI","EXCEL"]
categories = ["java"]
+++
In some situation, we need to use dependent drop-down list in Excel. For example, there're two drop-down list, parent cateogory list and subcategory list. The subcategory list depends on which item parent selected. How to make it through [Apache POI](https://poi.apache.org/)?

First, we need to figure out how to make it in Excel. We can use function **INDIRECT** and **VLOOkUP** to achieve it. [Create Dependent Drop Down Lists](http://www.contextures.com/xlDataVal02.html) is a good tutorial to show you how to do it in Excel.
Then, we need to figure out how to build this formula in POI. We can't set the explicit validation list as the list is from database and can be populated.

So we need to generate the parent list and subcategory list in sepecified column first. When we create the subcategory list, we need to add extra field to indicate which parent it belongs. Something likes this in Excel.

| Parent Category|   | SubCateogry| Belongs  |   |
|----------------|---|------------|----------|---|
| Parent A       |   | Sub A      | Parent A |   | 
| Parent B       |   | Sub B      | Parent A |   |
| Parent C       |   | Sub C      | Parent B |   |
|                |   | Sub D      | Parent C |   |

For parent list, we create drop-down list directly. For example,
***dvConstraint = DVConstraint.createFormulaListConstraint("$A$1:$A$3");***
It will set validation list from A1 to A3.
When you select "Parent A", we need to look up the subcategoires belongs to it.
```java
String searchTable = "$" + sublcategoryCol + "$" + (rowStart + 1) + ":" + "$" + rangeCol + "$" + (rowEnd + 1);
String lookupValue = "$" + valueCol + "$" + (valueRowIndex + 1);
String formula = "VLOOKUP(" + lookupValue + "," + searchTable + ",2,FALSE)";
```
The *searchTable* is a table range it will look up for the parent.
The *lookupValue* is the selected value of parent list.
After looking up the subcategories, we can use **INDIRECT** to add refrence for subcategory cell.
```java
String formula = "INDIRECT(" + formula + ")";
```
