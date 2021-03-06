# DeepVirFinder: Identifying viruses from metagenomic data by deep learning

Version: 1.0

Authors: Jie Ren, Kai Song, Chao Deng, Nathan Ahlgren, Jed Fuhrman, Yi Li, Xiaohui Xie, Ryan Poplin, Fengzhu Sun

Maintainer: Jie Ren renj@usc.edu


## Description

DeepVirFinder predicts viral sequences using deep learning method. 
The method has good prediction accuracy for short viral sequences, 
so it can be used to predict sequences from the metagenomic data.

DeepVirFinder significantly improves the prediction accuracy compared to our k-mer based method VirFinder by using convolutional neural networks (CNN).
CNN can automatically learn genomic patterns from the viral and prokaryotic sequences and simultaneously build a predictive model based on the learned genomic patterns. 
The learned patterns are represented in the form of weight matrices of size 4 by k, where k is the length of the pattern. 
This representation is similar to the position weight matrix (PWM), the commonly used representation of biological motifs, 
which are also of size 4 by k and each column specifies the probabilities of having the 4 nucleotides at that position.
When only one type of nucleotide can be chosen at each position with probability 1, the motif degenerates to a k-mer. 
Thus, the CNN is a natural generalization of k-mer based model. 
The more flexible CNN model indeed outperforms the k-mer based model on viral sequence prediction problem.


## Dependencies

DeepVirFinder requires Python 3.6 with the packages of numpy, theano and keras.
We recommand the use [Miniconda](https://conda.io/miniconda.html) to install all dependencies. 
After installing Miniconda, simply run


    conda install python=3.6 numpy theano keras



## Installation

Download the package by 

    git clone https://github.com/jessieren/DeepVirFinder
    cd DeepVirFinder
    
    
## Usage

The input of DeepVirFinder is the fasta file containing the sequences to predict, 
and the output is a .txt file containing the predicted score and p-value for each of the input sequences. 
The higher score or lower p-value indicate higher likelihood of being a viral sequence. 
The p-value is compuated by comparing the predicted score with the null distribution for prokaryotic sequences. 

The output file will be in the same directory as the input file by default. Users can also specify the output directory by the option [-o].
The option [-l] is for setting a minimun sequence length threshold so that sequences shorter than this threshold will not be predicted.
The program also supports parallel computing. Using [-c] to specify the number of threads to use. 


    python dvf.py -i INPUT_FA [-o OUTPUT_DIR] [-l CUTOFF_LEN] [-c CORE_NUM]


#### Options
      -h, --help            show this help message and exit
      -i INPUT_FA, --in=INPUT_FA
                            input fasta file
      -o OUTPUT_DIR, --out=OUTPUT_DIR
                            output directory
      -l CUTOFF_LEN, --len=CUTOFF_LEN
                            predict only for sequence >= L bp (default 1)
      -c CORE_NUM, --core=CORE_NUM
                            number of parallel cores (default 1)


## Examples

#### Predicting the crAssphage genome

    python dvf.py -i ./test/crAssphage.fa -o ./test/ -l 300
    
     
#### Predicting a set of metagenomically assembled contigs
    
    python dvf.py -i ./test/CRC_meta.fa -l 1000 -c 2
    
    

## Notes

#### If you would like to compute q-values (false discovery rate), please use the R package "qvalue". 

  To install the package "qvalue" in R:

  ```
  # try http:// if https:// URLs are not supported; it also checks for out-of-date packages
  source("https://bioconductor.org/biocLite.R")
  biocLite("qvalue")
  ```
  
  To compute the q-values, load the package and call the function 'qvalue'. For example, 

  ```
  # load the package qvalue
  library(qvalue)

  # read the prediction results
  result <- read.csv("./test/CRC_meta.fa_gt1000bp_dvfpred.txt", sep='\t')

  # estimate q-values (false discovery rates) based on p-values
  result$qvalue <- qvalue(result$pvalue)$qvalues

  # sort sequences by q-value in ascending order
  result[order(result$qvalue),]
  ```



<!-- Copyright and License Information
-----------------------------------

Copyright (C) 2017 University of Southern California

Authors: Jie Ren, Fengzhu Sun

This program is freely available under the terms of the GNU General Public (version 3) as published by the Free Software Foundation (http://www.gnu.org/licenses/#GPL) for academic use. 

Commercial users should contact Dr. Sun at fsun@usc.edu, copyright at the University of Southern California. 

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details. -->

<!--You should have received a copy of the GNU General Public License along with this program. If not, see http://www.gnu.org/licenses/.-->

