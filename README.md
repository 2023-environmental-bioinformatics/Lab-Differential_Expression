# Lab_GeneExpression

First, hop onto Poseidon, navigate into your user folder for the class, and make a new directory called `diffex` to play in for this lab. You don't need to copy anything from git - we will get a publicly-available example data set through R.

Request some interactive space on the HPC:\
`srun -p compute --time=01:00:00 --ntasks-per-node=1 --mem=10gb --pty bash` 

Now, let's set up a conda environment to play in:

```
conda create --name lab_de
conda activate lab_de
conda config --add channels conda-forge
conda config --set channel_priority strict
conda install -c conda-forge r-base
conda install -c anaconda jupyter
```

This will install R in a conda environment. Nearly all gene expression analysis programs work in R, a higher-level coding language that is particularly good for statistics, data management, and plotting. Much like python, a lot of the good stuff in R is done through "add-on" modules for more specialized tasks. In R, these are called packages.

*BONUS R factoid for baseball lovers: you can install the entire Sean Lahman Baseball Database in R if you want to try your hand at sabermetrics. The package is called `Lahman`, and it contains statistics from 1871-present.*

First, we need to install some packages we'll want to work with: `DESeq2` (which is used for differential expression), and `pasilla`, which contains an example gene expression data set. You can install R packages in conda, which we'll do here for simplicity. However, there's a more standard approach to installing R packages using the R `install.packages()` command. This approach is outlined briefly in the `r_install-packages.md` file in this repo.

Install `DESeq2` and `pasilla` through conda:

```
conda install -c conda-forge r-matrix
conda install -c bioconda bioconductor-deseq2
conda install -c bioconda bioconductor-pasilla
```

Note: We're installing Matrix first, since it's required by DESeq2 but does not install properly as part of the DESeq2 installation. (One of the many compatibility mysteries of R.)

Open `R` interactively by typing:\
`R`

Your prompt should change to a greater-than sign:\
`>`

Once packages are installed, you need to load them in your environment (or script) in order to use them. R packages are loaded with this syntax: `library(PACKAGE_NAME)`. Like so many things in UNIX, package names are case-sensitive. Now, load the DESeq2 library that you previously installed:

```
library(DESeq2)
library(pasilla)
```

Now, we'll follow along with the DESeq2 vignette available online here:\
http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html

If you want to make any figures, you can save them to pdf this way:

```
pdf(file = "FILENAME.pdf")
COMMAND THAT MAKES THE FIGURE
dev.off()
```

IMPORTANT NOTE: Do NOT directly enter any R commands that output figures on Poseidon. Always redirect the output to a file, as we are doing with a pdf here. By default, R prints to screen...and  the HPC doesn’t have a screen. (It will not print to your local computer, since it’s not running locally.) If you do directly enter a command that outputs a figure, you will get nothing more than a sad, empty file named `Rplots.pdf`. If you're using a more complex command with multiple outputs, there are ways to get around this issue, but for now, please just redirect anything you want to look at to pdf.
