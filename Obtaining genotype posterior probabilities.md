# Obtaining genotype posterior probabilities with ANGSD

Working with ancient DNA and low coverage samples can lead to uncertainty in genotype calls. One way to account for  
this uncertainty in the data is to maintain it in downstream analses and to weight any calculations by the genotype posterior  
probabilities, rather than doing a traditional genotype call. This can be done using ANGSD to obtain genotype likelihoods and  
then transforming these into posterior probabilities.

## Running ANGSD

### Step 1: Creating a sites file for ANGSD (if running on a subset of the genome)

ANGSD allows filtering by both chromsome (requires an indexed bam) or by particular sites. This requires an angsd file which  
can easily be created from a bed file using awk and then indexed with angsd http://www.popgen.dk/angsd/index.php/Sites 

1. Use awk to convert bed file with co-ordinates into an angsd site file:
```linux
awk '{print $1 "\t" $2+1 "\t" $3}' input.bed > angsd.file
```
2. Index the file:
```linux
angsd sites index angsd.file
```

### Step 2: Running ANGSD

ANGSD has various options for obtaining genotype likelihoods, all explained on the software's [wiki page](http://www.popgen.dk/angsd/index.php/Genotype_Likelihoods). Below is 
an example command using a sites files and specifying a region (remember to index the bam first if using the -r option). This  
example is using a GATK model (-GL 2) and outputting all 10 genotype likelihoods in a tab separated format (-doGlf 4).

1. Specify files and run ANGSD with parameters of choice:
```linux
BAM=test_dog.bam
SITES=dog_angsd.file
REF=Canis_familiaris.CanFam3.1.dna.toplevel.fa 

angsd -i $BAM -sites $SITES -r chr10 -out $OUT -minQ 20 -minMapQ 20 -remove_bads 1 -ref $REF -GL 2 -doGlf 4
```

## Transforming genotype likelihoods into genotype probabilities

The step above will create a .glf file with a line for every position, with genotype likelihoods for each of the ten genotypes  
presented as a likelihood ratio. The most likely genotype has a score of 0, and every other genotype's likelihood is a ratio
to this value. 

(Add in example here)

These likelihood ratios can be converted to posterior probabilities using the following equation: 
For a more in depth explanation and proof, see here. (Link)

(Add in equation here)

One way of implementing this equation is using python. 


