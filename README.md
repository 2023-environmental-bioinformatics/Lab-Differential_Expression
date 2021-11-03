# Lab_GeneExpression

First, hop onto Poseidon and clone this repo. All that's in it is this readme and a .yml file to set up your conda environment - we will get a publicly-available example data set through R.

Request some interactive space on the HPC:\
`srun -p compute --time=01:30:00 --ntasks-per-node=1 --mem=10gb --pty bash` 

Now, let's set up a conda environment to play in using the provided .yml file. It can be complicated to install R in a conda environment, so I'm giving you a file that will let you work with R in a jupyter notebook.

```
conda env create -f R_Jupyter.yml
conda activate r_jupyter
```

Before moving over to Jupyter, we're going to install the packages we need in R first. Open R interactively by typing:\
`R`

Your prompt should change to a greater-than sign:\
`>`

First, we need to install some packages we'll want to work with: `DESeq2` (which is used for differential expression), and `pasilla`, which contains an example gene expression data set. The standard approach to installing R packages uses the R `install.packages()` command. However, some R packages are available on a more specialized sciencey corner of R called Bioconductor (similar to biopython), and these need to be installed somewhat differently. If `install.packages()` says that a package doesn't exist, search for it online - it is likely installed via Bioconductor instead.

Install `DESeq2` and `pasilla` (data package) through Bioconductor. You'll need to first install `BiocManager` (the Bioconductor installer).

```
install.packages("BiocManager")
BiocManager::install("DESeq2")
BiocManager::install("pasilla")
```

Now that we've installed what we need, let's open up a jupyter notebook to play in R. (You *could* install these packages in a notebook, but I prefer the direct approach since you get a lot more output info that helps you figure out if things are installing correctly - and when they are finished.)

If you don't remember your jupyter password, reset it:

```
jupyter notebook password
```

From your `Lab_Differential-Expression` folder, spin up a jupyter notebook on the HPC:

```
jupyter notebook --no-browser --port=8888
```

...and from a new **local** terminal window, connect your computer to your new notebook:

```
ssh -N -f -L localhost:8888:localhost:8888 USERNAME@poseidon-[l1 or l2].whoi.edu
```

Remember to change the `8888` to whatever port you were assigned, add your username, and pick the appropriate login node (`l1` or `l2`).

Now, open a new browser window, connect to jupyter at the appropriate port, and enter your password:

```
localhost:8888
```

Open a new notebook in `R`.

Nearly all gene expression analysis programs work in R, a higher-level coding language that is particularly good for statistics, data management, and plotting. Much like python, a lot of the good stuff in R is done through "add-on" modules for more specialized tasks. In R, these are called packages.

*BONUS R factoid for baseball lovers: you can install the entire Sean Lahman Baseball Database in R if you want to try your hand at sabermetrics. The package is called `Lahman`, and it contains statistics from 1871-present.*

First, we need to install some packages we'll want to work with: `DESeq2` (which is used for differential expression), and `pasilla`, which contains an example gene expression data set. The standard approach to installing R packages uses the R `install.packages()` command. However, some R packages are available on a more specialized sciencey corner of R called Bioconductor (similar to biopython), and these need to be installed somewhat differently. If `install.packages()` says that a package doesn't exist, search for it online - it is likely installed via Bioconductor instead.

Install `DESeq2` and `pasilla` (data package) through Bioconductor. You'll need to first install `BiocManager` (the Bioconductor installer) and `Matrix` (required for DESeq2 to run but inexplicably not packaged with it) with the standard approach.

```
install.packages("BiocManager")
install.packages("Matrix")
BiocManager::install("DESeq2")
BiocManager::install("pasilla")
```

Note: We're installing Matrix first, since it's required by DESeq2 but does not install properly as part of the DESeq2 installation. (One of the many compatibility mysteries of R.)

While jupyter works well for running R, lots of folks prefer to run R locally (not on the HPC) because it can be irritating to install R packages in conda. If you prefer this approach, install `R` on your local computer and open it interactively by typing:\
`R`

Your prompt should change to a greater-than sign:\
`>`

For a third alternative, RStudio is a very popular interface for R that provides a nice visual interface. (RStudio can only be run on your local computer and is not compatible with the HPC.)

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
