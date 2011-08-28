BUILDDIR = _build
TEXDIR = _build/tex
HTMLDIR = _build/html
PDFDIR = _build/pdfs
PAPERDIR = ../output
TEMPLATES = _templates

BUILDTMPL = ./build_template.py
TEX2PDF := cd $(TEXDIR) && pdflatex -interaction=batchmode

.PHONY: front-pdf proceedings papers toc clean

all: clean proceedings

clean:
	rm -rf $(PAPERDIR)/* $(BUILDDIR)/*

$(TEXDIR): 
	mkdir -p $@

$(HTMLDIR):
	mkdir -p $@

$(TEXDIR)/cover.pdf: $(TEXDIR)
	inkscape --export-dpi=600 --export-pdf=$@ $(TEMPLATES)/cover.svg

$(HTMLDIR)/%.html: $(HTMLDIR)
	$(BUILDTMPL) $(TEMPLATES)/$(@F).tmpl > $@

$(TEXDIR)/%.tex: $(TEXDIR)
#	$(BUILDTMPL) $(TEMPLATES)/$(@F).tmpl > $@
	$(BUILDTMPL) $(@F)

$(TEXDIR)/%.pdf: $(TEXDIR)/%.tex
	($(TEX2PDF) $(<F) 1>/dev/null)

cover-pdf: $(TEXDIR)/cover.pdf
copyright-pdf: $(TEXDIR)/copyright.pdf
organization-pdf: $(TEXDIR)/organization.pdf
organization-html: $(HTMLDIR)/organization.html

front-pdf: cover-pdf copyright-pdf organization-pdf
front-html: organization-html


papers:
	./make_all.sh

proceedings: front-pdf papers $(TEXDIR)/proceedings.tex
	($(TEX2PDF) proceedings 1>/dev/null)
	($(TEX2PDF) proceedings 1>/dev/null)
	cp $(TEXDIR)/proceedings.pdf $(PDFDIR)/proceedings.pdf 

proceedings-html: front-html proceedings
	ln -s ../pdfs $(HTMLDIR)/pdfs
	cp $(TEMPLATES)/toc.css $(HTMLDIR)/.
	build_template.py $(TEMPLATES)/toc.html.tmpl $(BUILDDIR)/toc.json > $(HTMLDIR)/index.html