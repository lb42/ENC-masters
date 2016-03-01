ECHO=
FRENCHIFY=/home/lou/Public/tei-fr/trunk/Talks/frenchify.xsl
TALKS= \
talk-01-init \
talk-02-tei \
talk-03-cards \
talk-04-tagdocs \
talk-05-perso

EXERCISES= \

default: pdfslides

exercises:
	for i in $(EXERCISES) ; do \
		teitopdf $$i.xml; \
	done

slides: pdfslides 

pdfslides:  
	for i in $(TALKS) ; do \
	saxon $$i.xml $(FRENCHIFY) -o:$$i-fr.xml; \
	teitoslides --profile=tei $$i-fr.xml; \
		mv $$i-fr.xml.slides $$i.pdf; \
	done
clean:
	-rm -f *.aux
	-rm -f *.log
	-rm -f *.nav
	-rm -f *.out
	-rm -f *.snm
	-rm -f *.tex
	-rm -f *.toc
	-rm -f *.vrb
	-rm -f *.eps *.dvi
	-rm -f *.xhtml
	-rm -f *~


all:
	xmllint --xinclude talks.xml > mutecxsl.xml
	teitoepub --profile=oucs --odd mutecxsl.xml
	teitodocx --profile=oucs mutecxsl.xml
	rm -f Work.zip
	zip -r Work `find Work -type f | grep -v .svn`

handout: 
	cat booklet-top.txt > booklet-xi.xml
	echo '<?tex \\newpage ?><div><head>Exercises</head>' >> booklet-xi.xml
	for i in $(EXERCISES); do \
		echo "<div>" >> booklet-xi.xml; \
		grep "<title>" $$i.xml | head -1 | sed 's/title>/head>/g' >> booklet-xi.xml; \
		echo "<xi:include href=\"$$i.xml#xmlns(t=http://www.tei-c.org/ns/1.0)xpointer(/t:TEI/t:text/t:body/t:*)\"/>" >> booklet-xi.xml; \
		echo "</div>" >> booklet-xi.xml; \
		echo '<?tex \\newpage ?>'>> booklet-xi.xml ; \
	done
	echo "</div>" >> booklet-xi.xml
	echo '<?tex \\newpage ?>'>> booklet-xi.xml
	cat booklet-end.txt >> booklet-xi.xml
	xmllint --xinclude booklet-xi.xml > booklet.xml
	saxon booklet.xml booklet.xsl > booklet.tex
	perl -p -i -e 's+.color{red}++g' booklet.tex
	echo perl -p -i -e 's+\\maketitle++g' booklet.tex
	pdflatex booklet
	pdflatex booklet
	pdflatex booklet
	mv booklet.pdf exercices.pdf

validate:
	-for i in $(TALKS) ; do onvdl ../teitalks.nvdl `basename $$i .xml`.xml \
	 | grep -v ': error: unfinished element$$' \
	 | grep -v ': error: unfinished element .* required to finish the element$$'; done


