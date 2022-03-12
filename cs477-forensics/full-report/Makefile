report.pdf: report.md $(wildcard supporting/*)
	pandoc -s report.md -o report.pdf -V geometry:margin=1in --highlight=tango --citeproc -F pantable -F pandoc-include
