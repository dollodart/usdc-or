# Project Description

This is a style that is derivative of the article class to be used for legal
proceedings in the United States District Court, District of Oregon. It likely
follows most of the local rules for other federal district courts. Some control
sequences must be defined before loading the style, and here that is
accomplished by the line `\input{test-params}` to input the macros contained in
test-params.tex before `\usepackage{usdc-or}`.

This package automates the case caption, counsel information (a macro should be
provided to typeset the counsel information), signature, and certificate of
service. It can be used with the legbib package, also by this author, to
automate legal citations, including those to exhibits and to counsel (an
@counsel entry is defined in a .bib used to typeset the counsel contact
information by Biblatex). The LaTeX article class and other standard (tex-live)
packages (hyperref, fancyhdr) are used to automate footers, page numbering,
table of contents, section and paragraph references (including links), and
backlinks.

## Document Object Model

A dot file showing an example document format is at doc/dom.dot. The document
object model here assumes that all text is placed in a paragraph or
subparagraph. But any depth of section (section, subsection, and subsubsection)
can have a paragraph, and possibly then a subparagraph. There must be at least
a section for any paragraph. 

The references to sections are arabic numbers, the paragraphs alphabetical, and
the subparagraphs roman, with delimitation by dots between sections and opening
and closing parantheses around paragraphs and subparagraphs. Therefore the
general reference is, in an approximate regex,
`\d+\.(\d+\.)?(\d+\.)?\([a-z]\)(\([romannumeral]\))?` (see
https://stackoverflow.com/questions/267399 for regex for roman numerals). The
named destinations are given the same text as what is typeset to allow for easy
cross-referencing.

## Redaction

Redaction is often required in legal proceedings where there is protected
information, examples being private information of individuals and commercially
sensitive information of businesses. Generally redaction is done *a posteriori*
using a pdf editor that removes the text data and puts a black stripe in its
place. There is an advantage in *a priori* redaction, where protected
information in the text is never typeset. This poses some technical
difficulties for a program which does as much complicated computing as TeX does
if one wants to do what might be called conservative *a priori* redaction,
where there is no difference in the positions of the unredacted text between
redacted and unredacted documents.  The advantage of conservative *a priori*
redaction is that jurists can then make equivalent references to sealed and
unsealed documents in terms of positional locators (page and line numbers). But
even if you replace all characters with boxes of the equivalent width, ligature
and hyphenation will lead to different outputs of the unredacted text. The only
way to accomplish conservative redaction, to the author's knowledge, is to use
LuaLaTeX to have TeX construct the boxes for labeled characters, then remove
the characters from the constructed boxes. The different *a priori* redaction
techniques are reviewed at https://tex.stackexchange.com/questions/596971,
including a possible virtual font solution.  Here, nonconservative *a priori*
redaction is supported. The command \litredact replaces characters with
equivalent width white space (\litredact), and unless used very often or on
large segments of text should be approximately conservative. There is also the
highly nonconservative *a priori* redaction command \redact, that indicates how
much redacted text there was by a character count or replaces the text to be
redacted with a description of what was redacted.

Note that perfect redaction can be a security compromise for non-monospaced
fonts, because an adversary could determine based on the dimensions of the
unredacted text what sequence(s) of characters could have been used. 

# TODOs

- Add cross-referencing (in \externalcitenameddest) to named destinations,
  which are the pdf equivalent of HTML fragments. All pdf viewers tested
  (Okular, Evince, PDFStudio) don't support links to named destinations, however.
- Add conservative redaction (LuaLaTeX).
- Add redaction for section headings (protected commands).
- Add width-control for page marks to allow section headings (protected commands).
