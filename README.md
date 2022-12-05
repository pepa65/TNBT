# Thai New Buddhist Translation
* Content: the New Testament books of the Bible in a Thai translation
  that is meant to be more accessible to Thai Buddhists.
* Author: Banpote Wetchgama <banpote1947@gmail.com> +66(0)615347570
* Copyright 2014-2022
* License: CC BY-SA 4.0 - Creative Commons Attribution–ShareAlike

The `.doc` documents in `doc` are the author's original files, and the author
expressly allows them to be altered according to need or wishes, as long as
the results keep being shared.

This repository contains all the html, pdf and epub files that it helps produce
already in their respective `html`, `pdf` and `epub` directories.
This repository itself can be cloned and modified, the `.tx` files can be modified
in accordance with the `CC BY-SA 4.0` license, the tool files are licenced under
`GPLv3` in a similar spirit and can be modified according to need or wishes as well.

All files can be (re)produced after modifications by: `./mkall`

## The .doc source files
* The `.doc` files in the `doc` directory are the original files as produced by the author.
  All subsequent content counts as derivative works and must be licensed `CC BY-SA 4.0`.
* These source files have been processed into bare-bones `.tx` files:
  - Using the `unoconv` package, the docs were converted to plain text: `unoconv -f txt doc/*.doc`
  - From the plain text files, anything that is not Bible text was stripped away:
    mainly introductions and notes at the end, and a few verse references in the text.
    Some obvious minor corrections were made. The use of quotes was revised and made consistent.
  - Section headers were detected based on length (and no verse numbers present).
  - Lines to be indented (poetry or quotations) needed to be marked.
  - Non-verse numbers needed to be marked. Check with:
```
for f in tx/*.tx
do echo -ne "$f "
	while read -r l
	do t=${#l} v=${l##* } d=${#v}
		((t != v*3-8-d)) && echo "$l"
	done <<<"$(tail +4 "$f" |
		grep -v '^[1-9][0-9]*$' |
		grep -v '^_' |
		grep '[1-9][0-9]*' |
		tr $'\n' ' ' |
		sed 's/,[1-9][0-9]*//g' |
		sed 's/\b[^0-9]*/ /g' |
		sed -z 's/ 1 /\n1 /g')"
done
```

## The .tx source files
The files in the `tx` directory are the result of the processing of the source documents.
They are used as the fundamental input for all the utility applications:
`mkhtm`, `mkepub`, `mkpdfs` and `mkall`.

### .tx Format
* First 3 lines:
  - USFM book number (space) Abbreviation for the translation
  - Bible book name
  - Translation name (comma)(space) Author (space) <Email> (space) Telephone (space) [License]
* Chapter on separate line
* Header: `_` first char
* Indent: `.` first char
* Non-verse numbers are prefixed with `,`

## A standalone New Testament webpage
All the books of the New Testament in a standalone html page through: `./mkhtm`
(This uses `mkidx` to add an index to the non-indexed html page `html/TNBT-NT.htm`.)
The resulting `html/TNBT-NT.html` can be served as a single-page TNBT New Testament html page.

## Epub file for e-readers: TNBT-NT.epub
* Produce an epub3 file with the whole New Testament: `./mkepub`

## PDF files: TNBT-NT.pdf and individual Bible books
* Packages needed: `swath` (Thai word breaks) and `weasyprint` (make PDFs from html).
* Insert wordbreaks into the html file: `swath -b X -u u,u <html/TNBT-NT.htm |sed 's/X/<wbr>/g' >NT.htm`
* Produce simple PDF: `weasyprint -s weasyprint.css NT.htm pdf/TNBT-NT.pdf`
* A single Bible book (eg. John) can be made into a .pdf with `mkhtm` by:
  `mkhtm JHN |swath -b X -u u,u |sed 's/X/<wbr>/g' |weasyprint -s weasyprint.css - pdf/NTBT-JHN.pdf`

## Presentation website
* The files in the `html` directory serve the introductory page to this repo, particularly:
  `html/index.html`, `scroll.jpg` for the background and `favicon.png` as the logo/favicon
  together with `html/nt.htm` and `html/nt.html` (which are produced by `mksrv`).
