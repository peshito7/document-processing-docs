---
title: Getting Started
page_title: Getting Started
description: Getting Started
slug: radspreadstreamprocessing-getting-started
tags: getting started
published: True
position: 2
---

# Getting Started

This article will get you started in using the **RadSpreadStreamProcessing** library. It contains the following sections:

* [Assembly References](#assembly-references)

* [Supported Formats](#supported-formats)

* [Create a Spreadsheet Document](#create-a-spreadsheet-document)

## Assembly References

In order to use RadSpreadStreamProcessing in your project, you will need to add a reference to:

* **Telerik.Documents.SpreadsheetStreaming**

## Supported Formats

With RadSpreadStreamProcessing you can create XLSX and CSV files. All you have to do is to specify the desired format when creating a workbook with the CreateWorkbookExporter() method. **Example 1** demonstrates how this could be achieved.

#### **Example 1: Specify export format**

{{region #region radspreadstreamprocessing-getting-started_0}}

	IWorkbookExporter workbook = SpreadExporter.CreateWorkbookExporter(SpreadDocumentFormat.Xlsx, stream);     
{{endregion}}


## Create a Spreadsheet Document

This section will explain how a document could be created. If you need more detailed information about the classes that export the different document elements, you can check the articles from the [Model section]({%slug radspreadstreamprocessing-model-workbook%}).

When creating a document with **RadSpreadStremProcessing**, you should have in mind that the order of filling the content is much important. To achieve the low resources usage, the library directly writes the necessary content in a stream and due to the format restrictions of the way the content is presented in the structure of the file, you have to create the desired elements in a sequence keeping the following consecution:


1. Create a Workbook 

2. Create a Worksheet

3. Create Columns [optional]

4. Create Rows (a worksheet must contain at least one row)

5. Create Cells [optional]

6. Merge Cells [optional]

**Example 2** shows how you could create a simple document.

#### **Example 2: Create a document**

{{region #region radspreadstreamprocessing-getting-started_1}}
	using (FileStream stream = File.OpenWrite("sample.xlsx"))
	{
	    using (IWorkbookExporter workbook = SpreadExporter.CreateWorkbookExporter(SpreadDocumentFormat.Xlsx, stream))
	    {
	        using (IWorksheetExporter worksheet = workbook.CreateWorksheetExporter("My sheet"))
	        {
	            worksheet.SkipColumns(1);
	            using (IColumnExporter column = worksheet.CreateColumnExporter())
	            {
	                column.SetWidthInPixels(80);
	            }
	
	            worksheet.SkipRows(3);
	            using (IRowExporter row = worksheet.CreateRowExporter())
	            {
	                row.SkipCells(3);
	                using (ICellExporter cell = row.CreateCellExporter())
	                {
	                    cell.SetValue("Merged cell.");
	                    cell.SetFormat(new SpreadCellFormat()
	                    {
	                        HorizontalAlignment = SpreadHorizontalAlignment.Center,
	                        VerticalAlignment = SpreadVerticalAlignment.Center
	                    });
	                }
	            }
	
	            using (IRowExporter row = worksheet.CreateRowExporter())
	            {
	                row.SetHeightInPixels(200);
	                using (ICellExporter cell = row.CreateCellExporter())
	                {
	                    cell.SetValue(123.456);
	                }
	
	                using (ICellExporter cell = row.CreateCellExporter())
	                {
	                    SpreadCellFormat format = new SpreadCellFormat()
	                    {
	                        NumberFormat = "dd/mm/yyyy",
	                        IsBold = true
	                    };
	                    cell.SetFormat(format);
	                    cell.SetValue(42370);
	                }
	            }
	
	            worksheet.MergeCells(3, 3, 6, 6);
	        }
	    }
	}
{{endregion}}

**Figure 1** shows the result of executing the code from **Example 2**.

#### Figure 1: The document created in Example 2
![](images/SpreadStreamProcessing-GettingStarted_01.png)


# See Also 
* [Rows]({%slug radspreadstreamprocessing-model-rows%})
* [Columns]({%slug radspreadstreamprocessing-model-columns%})
* [Cells]({%slug radspreadstreamprocessing-model-cells%})
* [Worksheets]({%slug radspreadstreamprocessing-model-worksheet%})
* [Workbook]({%slug radspreadstreamprocessing-model-workbook%})