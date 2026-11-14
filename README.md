# jbrquarto: Quarto Extension for the *Journal of Business Research*

A [Quarto](https://quarto.org/) extension that formats manuscripts for the *Journal of Business Research* (JBR), published by Elsevier. One `.qmd` file renders to three outputs:

| Format | Output | Use |
| --- | --- | --- |
| `jbrquarto-docx` | Word file | An editable source file that JBR accepts |
| `jbrquarto-latex` | `.tex` source of the PDF | An editable source file that JBR accepts; compile with pdfLaTeX (two runs, or latexmk) |
| `jbrquarto-pdf` | PDF typeset with pdfLaTeX | For sharing and checking the manuscript. JBR does not accept a PDF as a source file. Rendering the PDF also keeps the `.tex` file. |

Version 1.0.0. The formatting follows the journal's [Guide for Authors](https://www.sciencedirect.com/journal/journal-of-business-research/publish/guide-for-authors), which was read in full as it stood on 13 November 2026, except where noted under [Choices the guide leaves open](#choices-the-guide-leaves-open). [What remains with the author](#what-remains-with-the-author) lists the requirements of the guide that a formatting tool cannot meet.

## Installation

Start a new manuscript from the template:

```bash
quarto use template mmourali/jbrquarto
```

Or add the extension to an existing project:

```bash
quarto add mmourali/jbrquarto
```

## The files JBR asks for

JBR uses double anonymized review.

| File | Template | Mode |
| --- | --- | --- |
| Anonymized manuscript | `template.qmd` | `jbr-mode: manuscript` (the default) |
| Title page with author details | `title-page.qmd`, with the details in `_title-page.yml` | `jbr-mode: title-page` |
| Vitae: a short biography of each author | `vitae.qmd`, with the biographies in `_title-page.yml` | `jbr-mode: vitae` |
| Each figure as a separate file | Written when `template.qmd` is rendered | `jbr-figures: files`; see [Figures as separate files](#figures-as-separate-files) |
| Highlights (encouraged) | `highlights.qmd` | `jbr-mode: highlights` |

```bash
quarto render template.qmd     # manuscript: Word, PDF, and .tex
quarto render title-page.qmd   # title page
quarto render vitae.qmd        # vitae.docx
quarto render highlights.qmd   # highlights.docx
```

**Manuscript.** US letter, 1-inch margins, Times New Roman 12 point, double spaced, single column, with page numbers. The first page holds the title, the abstract, and the keywords. The text begins on the next page under the title, which the guide asks authors to repeat there without their names. The reference list begins on a new page, and each table and figure follows on a page of its own, because the guide asks for tables and references on separate pages at the end. The manuscript names no authors. A submission may not exceed 45 double-spaced pages, not counting the title and abstract pages; the guide adds that final accepted manuscripts typically should have fewer than 8,000 words, all inclusive.

**Title page.** The title; the author names; below them the affiliations, each marked with a lowercase superscript letter and given with its full postal address, including the country; the e-mail address and the full postal address of the corresponding author; acknowledgments; funding; the declaration of competing interest; and the CRediT authorship contribution statement. All of it is entered in `_title-page.yml`. An author who has moved since the work was done, or who was visiting at the time, keeps the affiliation where the work was done and adds a `present-address` or `permanent-address` line; it is printed as a note marked with a superscript numeral. The guide asks authors to complete Elsevier's declarations tool and to upload the Word file it produces, and it asks for the declaration of competing interests on the title page only when that file is not submitted; delete `competing-interests` from `_title-page.yml` if you upload the file.

**Vitae.** The guide asks for a biography of each author of at most 100 words, in an editable format, and for a passport-type photograph of each author as a separate figure. Write each biography as the `biography` of its author in `_title-page.yml`. `vitae.qmd` renders the biographies to `vitae.docx`, one paragraph for each author, beginning with the author's name in bold; the render warns when a biography exceeds 100 words. The photographs are yours to supply.

**Highlights.** JBR encourages highlights. They go in a separate, editable file with the word "highlights" in its name: 3 to 5 bullet points of at most 85 characters each, including spaces. List them under `highlights:` in `highlights.qmd`; the render warns when a limit is exceeded.

**Article mode** (`jbr-mode: article`, PDF and `.tex` only) is a publication-style preview for sharing drafts. It is not a submission format.

## Options

| Field | What the extension does |
| --- | --- |
| `title` | Printed centered and bold on the first page and again above the text |
| `abstract` | Printed under "Abstract." Warns above 150 words. |
| `keywords` | Printed after the abstract. Warns unless there are four to six. |
| `jbr-mode` | `manuscript` (default), `title-page`, `vitae`, `highlights`, or `article` |
| `declarations` | Statements printed in new sections before the reference list: `generative-ai` (under "Declaration of generative AI and AI-assisted technologies in the manuscript preparation process") and `data-availability`. On the title page: `funding`, `competing-interests`, and `author-contributions`. |
| `jbr-floats` | `end` (default) moves each table and figure to a separate page after the references and marks its place in the text. `inline` leaves them next to the text. |
| `jbr-figures` | `embedded` (default), `files`, or `separate`; see [Figures as separate files](#figures-as-separate-files) |
| `anonymous` | `true` (default). `false` prints the title page with the author details first, every statement before the references, and the vitae at the end. |

## Text

**Sections.** Sections are numbered 1, 1.1, 1.1.1, and each heading stands on its own line: `#` in bold, `##` and `###` in italics. The abstract, the declarations, the appendixes, and the references are not numbered. Write the main sections with one `#`. (If a document has no `#` heading at all, its `##` headings are taken as the main sections and every heading moves up one level.)

**Hypotheses.** Write each hypothesis in a `hypothesis` div. It is indented, and its label (the words up to the first colon) is set in bold.

**Equations.** Label display equations (`$$ ... $$ {#eq-model}`) to number them consecutively. In the text, `@eq-model` prints "Eq. (1)."

**Appendixes.** Give each appendix heading the `appendix` class and a letter. Tables, figures, and equations in appendixes are numbered separately: Table A.1, Fig. A.1, Eq. (A.1). The headings inside an appendix are numbered after it: A.1., A.1.1.

```markdown
# Appendix A. Survey items {.appendix}
```

## Tables and figures

Give each table and figure a label and type its title without a number. Captions print as "**Table 1.** Title" above a table and "**Fig. 1.** Caption." below a figure. In the text, `@tbl-descriptives` prints "Table 1" and `@fig-loyalty` prints "Fig. 1."

```markdown
| Variable         | M    | SD   |
|:-----------------|-----:|-----:|
| Brand loyalty^a^ | 4.21 | 1.02 |
| Influencer trust | 4.56 | 0.89 |

: Descriptive statistics for the study variables {#tbl-descriptives}

::: {.notes}
All variables were measured on seven-point scales.

^a^Average of four items.
:::
```

Write tables and figures where they belong in the text. The extension moves each to a page of its own after the references, tables first, and leaves a line such as "[Insert Table 1 about here]" in its place. The tables and figures of an appendix stay in the appendix. Place table notes below the table body, in a `notes` div; they are printed as typed. Tables are single spaced, with horizontal rules only and no shading. A figure caption should give a brief title and a description of the illustration. The template's figure is drawn with R (ggplot2); delete that chunk if you do not use R.

**Word output.** Each table is a native Word table, centered, with column widths that fit its contents. The alignment is written into the table itself, so it does not depend on the program that opens the file. (A table that holds citations, footnotes, images, or web links is written by pandoc in the ordinary way and takes its alignment from the table style of the reference document.) A numbered equation and its number are placed in a table without borders, the equation in the middle cell and the number in the right cell, so that the number stands at the right margin. An equation without a number is centered in a paragraph of its own. A table or figure without a note is followed by 12 points of space, because Word otherwise sets the next paragraph directly against it.

**Labels and column widths.** A table or figure that has a title but no label is printed without a number, in every output, and the render warns; a table without a title takes no number either. In a pipe table whose lines are long, the number of dashes under each column head sets that column's share of the width. If a share is too small for the longest word of its column, the extension sets the widths from the contents instead, so that no word is broken in the middle.

**Figures and tables with panels.** Quarto's layout options set several pictures in one figure. Give the figure a label; give each panel a caption and, if the text cites single panels, a label of its own:

```markdown
::: {#fig-studies layout-ncol=2}
![Study 1](study1.png){#fig-study1}

![Study 2](study2.png){#fig-study2}

Purchase intention by condition
:::
```

All three outputs set the panels side by side, each with its caption, "(a) Study 1," centered below it, and `@fig-study1` prints "Fig. 1a." In Word the panels stand in a table without borders. A table with several parts (a `#tbl-` div with a layout option that holds one table for each part) is set the same way, and so are the plots of a code chunk with `layout-ncol` and `fig-subcap`.

**Page breaks and tables from code.** A table that fits on a page is kept on one page with its title and its note, and a note stays on the page of its figure. Tables that a code chunk writes (with `knitr::kable()`, for example) are treated like tables typed in Markdown. If a chunk writes its table as a LaTeX `table` environment, as `kableExtra` and `modelsummary` can, the extension formats its title and note; the layout inside the table is the package's own.

## Figures as separate files

The guide asks for every figure as a separate file, named in a logical way (Figure_1, Figure_2, and so on), along with the manuscript. The `jbr-figures` option writes these files when the manuscript is rendered:

| Value | Manuscript | Figure files |
| --- | --- | --- |
| `embedded` (default) | Figures on their pages at the end | Not written |
| `files` | Figures on their pages at the end | Written |
| `separate` | The captions of the figures, listed under "Figure captions" at the end (with `jbr-floats: inline`, each caption stands in place of its figure) | Written |

The files go to a folder next to the document that is named after it: `template-figures/` for `template.qmd`. Each file is named after the number the figure carries in the manuscript: `Figure_1`, `Figure_2`, and `Figure_A1` for Fig. A.1. A figure with panels gives one file for each panel (`Figure_3a`, `Figure_3b`); the guide asks authors not to combine several images or graphs in one file.

Each output copies the image it uses. For a figure drawn by a code chunk, the Word output writes the PNG version and the PDF and `.tex` outputs write the PDF version, so a render of all formats leaves both in the folder. The guide recommends EPS or PDF files for vector drawings, and TIFF, JPG, or PNG files for bitmaps, with at least 300 dpi for photographs and 1,000 dpi for line drawings. The template sets `fig-dpi: 300`; raise it for line drawings that you submit as bitmaps. An image that is not a file on your computer (a web address) cannot be copied, and the render says so. The files are written again at every render, so do not edit them in that folder; files left over from figures that you have since removed are not deleted.

The guide does not say whether the manuscript itself should also show the figures. The template uses `files`, which keeps them in the manuscript. Use `separate` if the editorial office asks for a manuscript without the figures; it then lists their captions on one page, in place of a page for each figure.

## Citations and references

JBR accepts any consistent reference style at submission and applies its own style at the proof stage; that style is APA (7th edition), which the extension uses. `[@belk2013]` prints (Belk, 2013), and a citation in the sentence, `@bagozzi1988`, prints "Bagozzi and Yi (1988)." DOIs in the `.bib` file are printed at the end of each reference.

```markdown
# References

::: {#refs}
:::
```

If the document has no reference list, the extension adds one. Declarations are printed in new sections just before it.

In the `.bib` file, give an article that is in press `year = {in press}` and no volume (with `year = {forthcoming}`, the word is printed twice); enter a working paper as `@techreport` with `type = {Working paper}` and `institution`; and wrap a word in braces to protect its capital letters in a title (`{Facebook}`).

## Choices the guide leaves open

The guide asks for double spacing, 1-inch margins, and 12-point type. It numbers sections 1, 1.1, 1.1.1 and cites appendix items as Table A.1, Fig. A.1, and Eq. (A.1). It does not prescribe the rest of the look of a manuscript, and the extension makes these choices:

- **Page and type.** US letter paper and Times New Roman. The guide names no paper size and no typeface. For A4 in the PDF and `.tex` outputs, add `classoption: a4paper` to the document options; in the Word file, change the paper size in Word.
- **First pages.** The guide describes a title page with the title, the authors, and their affiliations; a page with the abstract and the keywords; and the text, which begins on page 3 under the repeated title. It also asks for the title page and the anonymized manuscript as separate files. The anonymized manuscript therefore begins with the page of the abstract and the keywords, which carries the title in place of the separate title page, and the text begins on the next page under the repeated title. With `anonymous: false`, the three pages follow each other as the guide describes them.
- **Counting pages.** The limit of 45 pages leaves out the title and abstract pages. In the anonymized manuscript, count from the page on which the text begins, the second page of the file.
- **Tables and figures at the end.** The guide's section on the journal asks for tables on separate pages at the end; its general section on tables allows them next to the text or at the end. The extension follows the first and treats figures in the same way. `jbr-floats: inline` leaves both next to the text.
- **Keywords.** The guide's section on the journal asks for four to six keywords; its general section on keywords allows 1 to 7. The render warns outside four to six.
- **Headings and captions.** A period follows each section number ("1. Introduction"); captions carry a bold label and a period ("Table 1.", "Fig. 1."); and every equation is cited in the form Eq. (1), in keeping with the journal's published articles.
- **Statements that name the authors.** The guide asks for funding sources to be disclosed and for a CRediT statement, without saying where they go. Because they identify the authors, the extension prints them on the title page, not in the anonymized manuscript.
- **Vitae.** The guide does not say in which file the biographies belong. Because they identify the authors, the extension writes them to a file of their own.
- **Data availability.** The guide encourages authors to deposit their research data and to cite and link the dataset in the article, or else to state why the data cannot be shared. The `data-availability` declaration prints such a statement in the manuscript for authors who want one there; it is optional.

## Checks during rendering

The render warns when the abstract exceeds 150 words; when there are fewer than four or more than six keywords; when a biography exceeds 100 words; and when the highlights are fewer than 3, more than 5, or longer than 85 characters. Each render of the manuscript also reports its number of words, all inclusive (the abstract, the text with its footnotes, tables, captions, notes, and appendixes, and the references), next to the guide's figure of 8,000 words for final accepted manuscripts. The extension cannot check the 45-page limit or the anonymity of the manuscript.

## What remains with the author

The guide asks for more than a formatting tool can supply. These points are the author's to check:

- **Other files.** The declaration of competing interests from Elsevier's declarations tool; a passport-type photograph of each author, as a separate figure; and supplementary material, if you have any.
- **Research data.** The journal follows Elsevier's research data guideline "Option B": authors are encouraged to deposit the data in a repository and to cite and link the dataset in the article, or to state why the data cannot be shared.
- **Studies with human participants.** The guide requires a statement on ethical approval and informed consent. It names no place for it; write it where it fits, for example in the method section, without details that identify the authors.
- **Generative AI.** Declare the use of AI tools in the manuscript (the `generative-ai` declaration prints the section the guide asks for). If AI tools created an image, the guide also asks for a disclosure in the caption of that image. No statement is needed when there is nothing to disclose.
- **Writing.** American or British English, not a mix of the two; no underlined or struck-through text unless it is scientifically relevant; references to sections by their numbers; and few abbreviations and acronyms, which the guide cautions against.
- **References.** Every work cited in the text appears in the reference list and the reverse; data references carry the tag [dataset]; preprints are marked as such.

## Files

| File | Role |
| --- | --- |
| `_extensions/jbrquarto/_extension.yml` | The three formats and their settings |
| `_extensions/jbrquarto/jbrquarto.lua` | Front matter, title page, vitae, highlights, declarations, tables and figures at the end, appendix numbers, the figure files, the word count, and the checks |
| `_extensions/jbrquarto/jbrquarto-post.lua` | Captions, section numbers and appendix numbers in Word, equation references, Word tables, and figures with panels (it runs after Quarto has numbered tables and figures) |
| `_extensions/jbrquarto/jbr-header.tex` | LaTeX preamble: fonts, spacing, headings, captions |
| `_extensions/jbrquarto/jbr-reference.docx` | Word styles |
| `_extensions/jbrquarto/apa.csl` | APA style (7th edition) |
| `_extensions/jbrquarto/tools/build_reference_docx.py` | Rebuilds the Word reference document (requires Python and python-docx) |
| `template.qmd`, `bibliography.bib` | Manuscript template and its references |
| `title-page.qmd`, `_title-page.yml` | Title page template and the author details |
| `vitae.qmd` | Vitae template (the biographies are in `_title-page.yml`) |
| `highlights.qmd` | Highlights template |

## Requirements

Quarto 1.4 or later (tested with Quarto 1.4.557, 1.5.57, and 1.11.9; the Word output was checked in Microsoft Word for Mac) and a TeX distribution with pdfLaTeX. The PDF and `.tex` outputs use the `mathptmx` (Times) and `helvet` packages, which come with TeX Live and TinyTeX; the Word output takes Times New Roman from `jbr-reference.docx`.

Greek letters and common mathematical signs can be typed as characters (α, χ², ≤, ≠, −, ×, →, ✓). In the PDF and `.tex` outputs, which pdfLaTeX typesets, Greek letters stand upright in the text, as they do in the Word file. pdfLaTeX has no type for other scripts (Cyrillic, Chinese, Arabic) or for a letter with a combining accent (a mean typed as an x followed by a combining macron); write such symbols as math, `$\bar{x}$`. The Word output takes any character.

## Resources

- [JBR Guide for Authors](https://www.sciencedirect.com/journal/journal-of-business-research/publish/guide-for-authors)

## License

The CSL file comes from the [Citation Style Language](https://citationstyles.org/) project and is licensed under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/). All other files are available under the [MIT License](LICENSE).
