Title: How To Rearrange Letters In A PDF
Date: 2013-03-08 22:06
Author: alex
Category: Technology
Slug: how-to-rearrange-letters-in-a-pdf

Recently I was tasked with modifying a PDF.  Specifically, I was asked
to take an existing PDF template and replace placeholder text with
dynamic content.  Since I've worked with PDFs before, I knew that it
might be somewhat difficult but hopefully not something I couldn't
handle given my prior experience.

THE PROCESS
-----------

The process, that was explained to me, was that I would receive a PDF
"template" that contained a background image and text embedded in the
PDF. What I need to do was to find a way to replace this hard-coded
template text with dynamic text from the database.

LIBRARIES
---------

I started by looking into all the PDF libraries I could find that might
help me. There was some mention of Perl scripts throughout my research
but that didn't seem to work for me. I eventually settled on a couple of
useful libraries. One was the
[TET](http://www.pdflib.com/products/tet/ "TET") (Text Extraction
Toolkit). It was very useful in determining the font, color, and
positioning of text within the PDF. Another useful library was
[PageCatcher](http://www.reportlab.com/software/reportlab-plus/ "PageCatcher")
by ReportLab. They had the ability to leverage an existing PDF as a
"canvas" and write text on it.

SOLUTION
--------

Unfortunately, the solution I proposed required 2 PDFs, one with the
hard-coded text and another that was a blank canvas to write on. I was
able to use the TET library to extract the text information from the
first PDF and then write my dynamic text on the second PDF.

There seems to be a possible solution to actually replace existing text
within a PDF. Another thing that I learned is that fonts within a PDF
are closely tied with the actual text and not necessarily the PDF in
general. This means that if you wanted to replace certain letters in a
PDF, you would also need another page in the PDF that has all the
letters in the alphabet as a source to be used to replace the existing
text.

