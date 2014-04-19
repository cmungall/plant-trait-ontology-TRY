
# This Makefile is for use by the to Release Manager.
#
# requirements: Oort (http://code.google.com/p/owltools/wiki/OortIntro)
#
# To make a release, type 'make release' in this directory

OBO = http://purl.obolibrary.org/obo

initial-build: to.obo
build: to.obo minidiff
release: to.obo diff

all: to.obo

# all release files are derived from the editors version.
# we use the default Oort setup (hermit as reasoner)
to.obo: to-edit.owl
	ontology-release-runner --catalog-xml catalog-v001.xml $< --allow-overwrite --asserted --simple && cp to-simple.obo to.obo	

# ----------------------------------------
# Imports
# ----------------------------------------
all_imports: imports/po_import.owl imports/pato_import.owl imports/chebi_import.owl imports/eo_import.owl imports/go_import.owl

KEEPRELS = BFO:0000050 BFO:0000051 RO:0002202 immediate_transformation_of

mirror/%.owl:
	owltools $(OBO)/$*.owl --remove-annotation-assertions -l --make-subset-by-properties -f $(KEEPRELS) --set-ontology-id $(OBO)/$*.owl -o $@
.PRECIOUS: mirror/%.owl

imports/%_import.owl: to-edit.owl mirror/%.owl
	owltools --use-catalog --map-ontology-iri $(OBO)/to/imports/$*_import.owl mirror/$*.owl $<  --add-imports-from-supports --extract-module -s $(OBO)/$*.owl -c --extract-mingraph  --set-ontology-id $(OBO)/go/extensions/$@ -o $@

imports/oba_import.owl:

# ----------------------------------------
# Diffs
# ----------------------------------------

# 
diff: to.obo
	cd diffs && touch to-diff && rm to-*diff* && make

# Minidiff: intra-release differences
minidiff: to.obo
	cd diffs  && touch to-diff && rm to-*diff* && make TGTS='html txt' SRC=../to.obo && mail -s to-diffs RELEASE-MANAGER@FOO.BAR < to-combined-diff.txt

