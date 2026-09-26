# HBB Gene Analyser
A Biopython tool for analysing the human hemoglobin subunit beta gene which is responsible for making beta-globin, and the site of the mutation that causes sickle cell disease. 
The tool loads reference sequence data directly from an NCBI dataset download and provides a menu of classic sequence analysis operations.

[**▶ Open and run in Google Colab**] (https://colab.research.google.com/drive/1WR20RguwX2vJv0atDAGGxI6tCg2ECpho?usp=sharing)

## What it does

The NCBI dataset for a gene includes several different sequence files, each including a different form of the same gene. This tool loads all of them and lets you choose which one to analyse, explaining what each is for:

| Sequence | What it is | Best used for |
|---|---|---|
| **Gene** | Full genomic sequence, including introns and regulatory regions | Composition stats (GC%, base counts) |
| **mRNA** | Mature transcript, introns removed, UTRs still included | ORF detection |
| **CDS** | Pure protein-coding sequence only (start codon to stop codon) | Translation, mutation screening |
| **Protein** | The reference amino acid sequence | Visual comparison against translated CDS |

### Available tools

1. Basic sequence info (ID, description, length, annotated features)
2. GC content
3. Nucleotide composition (A/C/G/T breakdown)
4. DNA → mRNA transcription
5. DNA/mRNA → protein translation
6. Reverse complement
7. Open Reading Frame (ORF) detection across all 6 reading frames
8. Motif / subsequence search
9. Sickle-cell mutation screening (HbS, Glu6Val at codon 6)
10. Melting temperature (Tm) estimation (Wallace rule)

## How to run it

1. Go to [NCBI Gene](https://www.ncbi.nlm.nih.gov/gene) and search **HBB** (or use [this direct link](https://www.ncbi.nlm.nih.gov/gene/3043)).
2. Under "Genomic regions, transcripts and products," use **Download → Datasets (gene)** to get the `.zip` package. Do not unzip it.
3. Open the notebook in Colab (link above), or clone this repo and run it locally with Jupyter.
4. Run the cells in order. When prompted, upload the `.zip` file you downloaded.
5. Choose which sequence type to analyse (gene / mRNA / CDS / protein) and pick a tool from the menu.

## Requirements
Install with:
pip isntall -r requirements.txt
(Already handled automatically in the first cell if running via Colab.)

## Limitations and design notes

- **Sickle-cell check requires the CDS.** The "codon 6" convention for the HbS mutation counts the *mature* protein after the initiator methionine is removed, but this is actually the 7th codon in the raw CDS sequence. Running this operation on the full gene or mRNA won't give meaningul reasults. 
- **ORF detection on genomic DNA** will report many small and irrelevant ORFs from intron sequences, it is the most meaningful when run on the mRNA or CDS.
- **Melting temperature** uses the simple Wallace rule, which is best suited to short (~20–25 bp) primer-length fragments rather than long sequences.

## What I learned

This project involved interacting with real NCBI reference data which provided an opportunity to learn how to handle different sequence types. There were issues with file encoding, and coordinate accuracy in the mutation screening which only became clear when validated against the actual reference protein sequence. Managing these issues reinforced how important it is to check that the results are accurate and not just check that the operations run without errors. 

