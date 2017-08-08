.. TraPS-VarI documentation master file, created by
   sphinx-quickstart on Sun Aug  6 19:35:08 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


.. image:: image/banner.png

.. toctree::
   :maxdepth: 2
   :caption: Contents:

**TraPS VarI** is a tool to map the single nucleotide polymorphism mutations recorded in **.vcf** (VCF 4.0) files to the protein topology and predict the effect of 
an individual-specific genotype on the presence or absence of membrane-proximal SH2 binding motifs [Y**Q]. Additionally, the algorithm also finds matches for 
Uniprot Protein Name entries with the records of the therapeutic target database (ttd) and DrugBank.

************
Installation
************
TraPS VarI will add itself as a module to python.

Requirements
************

* python 3.4 or newer
* MySQLdb module for python (MySQL-python)
* access to a MySQL database (InnoDB engine with spatial index support - v5.7 or higher)

.. warning:: MySQL library has to be configured manually to enable successful integration of TraPS-VarI database


How to Install
**************
Extract the package to the desired install location and run the installation script with the command:
			python install.py

and simply follow the instructions appearing on the commandline

*****
Usage
*****

python TraPS_VarI.py {−p=<CHR: POS> −m=<REF/ALT >| −f=<filename> [options]} [−assembly=<assemblyversion>]

* -p=chr:pos to look up a single mutation (i.e. A/T) at the position chr:pos (only with -m)
* -m=REF/ALT mutation to look up (only in conjunction with -p)
* -f= to run the script on a file in vcf format
* -fout file to save the result to (default is inputfile traps vari output)
* -filter use to omit all lines from the result that do not contain a transmembrane protein mutation
* -assembly=37/38 (which assembly to use, dbSNP is only supported for 38)


.. note:: The script will generate an outputfile namely traps_vari_output with the following appended columns:-
	
	1. Uniprot Entry name,
	2. Uniprot Entry ID,	
	3. Protein position, 
	4. Protein aminoacid change, 
	5. Mutation effect,
	6. STAT3 binding motif alterations (namely, CREATED, DESTROYED or PRESENT),
	7. Topology domain, 
	8. Hits in the TTD database and 
	9. Hits in the DrugBank database.

***************
How it works
***************
TraPS VarI processes the vcf file line by line. It takes the position, matches this against
coding reqions in the RefSeq database. It then matches the CDS to it’s appropriate
UniProt entry, modifies the CDS according to the mutation and retranslates the resulting
CDS. The effect of the mutation is derived from the difference between those two entries.
In it’s current version it only matches against UniProts main entries and not their
isoforms (support for this is planned). It also looks up the position and mutation in the
dbSNP dataset, if the mutation is contained there it adds the dbSNPid to the entry. It
also checks the found refseq and uniprot ids against the TTD and DrugBank databases.

*****************
Graphical Summary
*****************
.. image:: image/summary.png

***********
Source code
***********

https://gitlab.com/VJ-Ulaganathan/TraPS-VarI

Binaries for Linux and Windows
******************************

https://github.com/VJ-Ulaganathan/TraPS-VarI

***********************
Biological Significance
***********************

The presence of even a single STAT3 recruitment site in the the single-pass type I membrane proteins
significantly elevates the levels of tyrosine phosphorylation in STAT3 thereby enhancing the amplitude of STAT3 signaling. Depending on the cell/tissue-specific expression patterns
of the receptor variant, pleiotropic effects are expected in vivo.
Ubiquitous expression of STAT3-enhancing gain-of-function SNPs in the germline genome contributes to accelerated cancer progression & poor prognosis. However, cells are rendered sensitive in a 
genotype/allele-specific manner to growth inhibition by small molecule or RNA interference mediated inhibition of STAT3 signaling pathways (Ulaganathan et al, Nature 2015: https://doi.org/10.1038/nature16449).
Thus TraPS-VarI can serve as an important annotation tool for personalized medicine in examining the patient-specific genotyping dataset.


Example-1
*********
Identifying the STAT3-enhancing germline variant TMEM9 **p.Y122Y** which is found in about 31% of the general population
========================================================================================================================


.. image:: image/example-skt.png

step-1: obtaining input ".vcf" file (typically SNP genotyping or WGS datasets)
------------------------------------------------------------------------------
.. image:: image/getting-patient.data.gif

step-2: exectuing TraPS-VarI
----------------------------
.. image:: image/script-init.gif

progress
^^^^^^^^
.. image:: image/progress.gif

step-3: finding the STAT3-recruting receptor variants
-----------------------------------------------------
.. image:: image/output.gif








Support
*******

If you are having issues, please let me know:
ulaganat@biochem.mpg.de


