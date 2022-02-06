pres: pandoc.md
	pandoc -t revealjs -s pandoc.md -o pandoc.html --slide-level 2 --mathjax https://cdn.jsdelivr.net/npm/mathjax@2/MathJax.js

.PHONY: clean
clean:
	rm -f pandoc.html
