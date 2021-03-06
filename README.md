# CRUK Analysis with the SMP2 v2 app
# 1_CRUK.sh script
## Introduction
This script takes fastq files and runs the SMP2 app in the basespace environment for each of the tumour sample blood sample pairs. 


The functionality to download the required output files for further analysis and interpretation has been removed due to local
technical limitations. 


A CentOS or Ubuntu operating system is required with the Illumina BaseSpace Command Line Interface installed and 
configured to access the correct BaseSpace location and username. For further instructions see 
https://help.basespace.illumina.com/articles/descriptive/basespace-cli/.


### Required input files
  * The Illumina SampleSheet.csv with the desired project identifier for BaseSpace in the Experiment Name field.

  * Fastq pairs (read 1 and read 2) for each of the samples.

  * An optional text file called "not_bs.txt" containing the names of any samples on the Illumina SampleSheet.csv for which
analysis in BaseSpace with the SMP2 app is not required. This should be placed in the same location as the script.

  * An optional text file containing tumour normal pairs in the format <tumour_sample_id> <tab> <blood_sample_id> with each 
pair on a new line. This is required if the arrangement of samples in the Illumina SampleSheet.csv does not match the expected
order, which is tumour sample then normal sample for each patient in order. An example of this order is: S1 tumour sample for person 
1, S2 blood sample for person 1, S3 tumour sample for person 2, S4 blood sample for person 2, etc. This file can be located anywhere 
on the same computer as the script. The name of the full path to the file, the file name file and extension must be supplied as the 
third command line argument e.g. /path/to/file/pairs.txt.


### Caveats
The script requires project names to be unique. The project name is entered in the Experiment Name field of the Illumina SampleSheet.csv. 


Sample identifiers must be unique within a given project. Sample identifiers are entered in the Sample_ID fields of the Illumina 
SampleSheet.csv.


## Instructions for use
### Prerequisites
The Illumina CLI must be correctly set up to point to the required BaseSpace location. A config file with a different name to the default 
can be used by changing the name of the $CONFIG variable in the script.

The SMP2 app must have been imported. Instructions to import apps are available on the Illumina website.

### Changes to the script required for initial set up
  * Check that the fastqs are located in the location in the $FASTQFOLDER variable and check thet  the $CONFIG variable is set to the name
    of the correct config file for use of the SMP2 v2 app.

  * Ensure that the $APPID variable is set to the correct application id for the imported version of the SMP2 v2 app.


### Instructions for running the script
  * Pass the full path to the SampleSheet.csv file for the run to be analysed as the first command line argument. The assumption is 
  that this will be the path to the run and that the fastqs will be located within the 
  /data/archive/fastq/<RUN_ID>/Data/<SAMPLE_ID>/ 
  directory as they would be if they were generated using the Illumina bcl2fastq on the local cluster. 
  If this is not the location of the fastq files, the variable $FASTQFOLDER within the script should be changed to the
  location of the fastq files.

  * Pass the sample name of the negative control sample as the second command line argument.

  * If a manually created file containing the tumour blood pairs is required, place this file on the same computer as the script and
  files to be analysed. Pass the full path to the file, the name of this file and the extension as the third command line argument.
  e.g. /path/to/pairs/file/pairs.txt. 
  
  * If the automatic pair generation option is used (default), the automatically generated SamplePairs.txt file will be created in the
  same location as the SampleSheet.csv.

  * If there are samples which were run, and so are on the sample sheet, that are not required to be analysed using the SMP2 BaseSpace application, 
the names of these samples should be placed in a file called "not_bs.txt" with each name on a new line. The file "not_bs.txt"
should be placed in the same directory as the script.

#### Full example
bash 1_CRUK.sh /path/to/samplesheet/ NEGATIVECONTROL /path/to/pairs/file/pairs.txt

Note that the third argument is optional.

## Creating the tumour blood pairs file
If the sample sheet is not set up with the pattern tumour sample followed by paired blood sample for each patient sequentially, it is necessary
to manually specify the tumour-normal pairs.
Create a text file according to the following pattern with the sample names:

tumour1 tab blood1 newline


tumour2 tab blood2 newline


tumour3 tab blood3 newline


...and so on for each pair of samples belonging to each individual.

The text file can have any name. It can be placed anywhere on the same computer as the script and the full path to the file, the name of the 
file and the extension should be passed as the third command line argument.
