WORKDIR=''
BUILDDIR=output
FILENAME='ExamplePresentation.md'

github-action: revealJS powerpoint

revealJS:
	cd $(WORKDIR)
	mkdir $(BUILDDIR) -p
	pandoc $(FILENAME) \
	--citeproc \
	--from=markdown+tex_math_single_backslash+tex_math_dollars+raw_tex \
	--to=revealjs \
	--mathjax \
	--output=$(BUILDDIR)/$(FILENAME).revealJS.html \
	--self-contained \
	--lua-filter=revealjs-codeblock.lua \
  	--template template.html \
	-V highlightjs \
	-V highlightjs-custom-theme=styles/Custom_CSharp.css \
	--css=theme/CustomTheme.css
  
powerpoint:
	cd $(WORKDIR)
	mkdir $(BUILDDIR) -p
	pandoc $(FILENAME) \
	--from=markdown+tex_math_single_backslash+tex_math_dollars+raw_tex \
	--to=pptx \
	--mathjax \
	--output=$(BUILDDIR)/$(FILENAME).pptx \
	--self-contained
	--resource-path=Images
