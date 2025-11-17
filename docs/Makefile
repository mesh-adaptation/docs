# Makefile for building the Sphinx documentation for Animate, Goalie, Movement, and UM2N

# You can set these variables from the command line.
SPHINXOPTS    =
SPHINXBUILD   = sphinx-build
SPHINXAPIDOC  = sphinx-apidoc
SPHINXPROJ    = Documentation
SOURCEDIR     = source
BUILDDIR      = build
PYTHON        = $$(which python3)
PKGDIR        = $$($(PYTHON) -c 'import animate; import os; print(os.path.dirname(os.path.dirname(animate.__path__[0])))' -options_left 0)

# Directories where mesh adaptation packages are located
ANIMATE       = ${PKGDIR}/animate
MOVEMENT      = ${PKGDIR}/movement
GOALIE        = ${PKGDIR}/goalie
UM2N          = ${PKGDIR}/UM2N

help:
	@$(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

.PHONY: apidoc help clean copy_demos

GENERATED_FILES = source/demos

run_animate_demos:
	cd ${ANIMATE}/demos/ && \
	for FILE in *.py; do \
		echo "Running Animate demo $$FILE"; \
		${PYTHON} $$FILE; \
	done

run_goalie_demos:
	SKIP_DEMOS="time_partition.py gray_scott.py gray_scott_split.py"; \
	cd ${GOALIE}/demos/ && \
	for FILE in *.py; do \
		if ! echo "$$SKIP_DEMOS" | grep -wq $$FILE; then \
			echo "Running Goalie demo $$FILE"; \
			${PYTHON} $$FILE; \
		else \
			echo "Skipping Goalie demo $$FILE"; \
		fi; \
	done

run_movement_demos:
	SKIP_DEMOS="monge_ampere_3d.py"; \
	cd ${MOVEMENT}/demos/ && \
	for FILE in *.py; do \
		if ! echo "$$SKIP_DEMOS" | grep -wq $$FILE; then \
			echo "Running Movement demo $$FILE"; \
			${PYTHON} $$FILE; \
		else \
			echo "Skipping Movement demo $$FILE"; \
		fi; \
	done

source/demos: copy_demos

copy_demos:
	install -d source/demos
	cp ${ANIMATE}/demos/*.py source/demos
	cp ${ANIMATE}/demos/*.jpg source/demos
	cp ${ANIMATE}/demos/*.svg source/demos
	
	cp ${GOALIE}/demos/*.py source/demos
	cp ${GOALIE}/demos/*.jpg source/demos
	
	cp ${MOVEMENT}/demos/*.py source/demos
	cp ${MOVEMENT}/demos/*.jpg source/demos
	
	for FILE in source/demos/*.py; do \
		pylit -c $$FILE; mv $$FILE.txt $$FILE.rst; \
	done
	install -d $(BUILDDIR)/html/demos
	cp source/demos/*.py $(BUILDDIR)/html/demos
	cp source/demos/*.jpg $(BUILDDIR)/html/demos
	install -d $(BUILDDIR)/html/animate
	install -d $(BUILDDIR)/html/goalie
	install -d $(BUILDDIR)/html/movement
	install -d $(BUILDDIR)/html/UM2N
	mkdir -p $(BUILDDIR)/html/_images
	cp source/animate/images/*.jpg $(BUILDDIR)/html/_images
	cp source/goalie/images/*.jpg $(BUILDDIR)/html/_images
	# cp source/movement/images/*.jpg $(BUILDDIR)/html/_images

SPHINX_TARGETS = dirhtml singlehtml pickle json htmlhelp qthelp devhelp epub \
	latex latexpdf latexpdfja text man texinfo info gettext changes \
	xml pseudoxml linkcheck doctest coverage

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
$(SPHINX_TARGETS): Makefile
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

html: apidoc $(GENERATED_FILES)
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

apidoc:
	$(SPHINXAPIDOC) $$(python3 -c "import animate; import os; print(os.path.dirname(animate.__file__))") -o source -f -T
	$(SPHINXAPIDOC) $$(python3 -c "import goalie; import os; print(os.path.dirname(goalie.__file__))") -o source -f -T
	$(SPHINXAPIDOC) $$(python3 -c "import movement; import os; print(os.path.dirname(movement.__file__))") -o source -f -T
	$(SPHINXAPIDOC) $$(python3 -c "import UM2N; import os; print(os.path.dirname(UM2N.__file__))") -o source -f -T

clean:
	-git clean -fdx $(BUILDDIR)/html/
	-rm -rf $(BUILDDIR)/doctrees
	-rm -rf $(GENERATED_FILES)
