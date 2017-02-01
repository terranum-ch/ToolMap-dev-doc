# ToolMap developer documentation

This repository contains the ToolMap developper documentation

## Prerequisite

In order to generate the documentation, install the following:

multimarkdown: http://fletcherpenney.net/multimarkdown/
LaTeX:
 - OSX: http://www.tug.org/mactex/
 - Windows: https://miktex.org or http://www.tug.org/texlive/

## Build PDF documentation

Run the following:
`multimarkdown -t latex -b file.markdown`

Convert the generated latex to PDF with `texi2pdf`

## Build HTML documentation

Run the following:
`multimarkdown -t html -b file.markdown`

## Build DOCX from HTML

Install PANDOC : http://johnmacfarlane.net/pandoc

Run the following:
`pandoc -t docx -f html -o test.docx file.html`