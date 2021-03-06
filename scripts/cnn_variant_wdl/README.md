# gatk4-cnn-variant-filter

### Purpose :
These workflows take advantage of GATK's CNN tool which uses a deep learning 
approach to filter variants based on Convolutional Neural Networks. 

Please read the following post to learn more about the CNN tool: [Deep Learning in GATK4](https://github.com/broadinstitute/gatk-docs/blob/3333b5aacfd3c48a87b60047395e1febc98c21f9/blog-2012-to-2019/2017-12-21-Deep_learning_in_GATK4.md).

### cram2filtered.wdl
This workflow takes an input CRAM/BAM to call variants with HaplotypeCaller
then filters the calls with the CNNScoreVariant neural net tool using the filtering model specified.

The site-level scores are added to the `INFO` field of the VCF. The architecture arguments,
`info_key` and `tensor_type` arguments MUST be in agreement (e.g. 2D models must have
`tensor_type` of `read_tensor` and `info_key` of `CNN_2D`, 1D models must have `tensor_type` of
`reference` and `info_key` of `CNN_1D`). The `INFO` field key will be `CNN_1D` or `CNN_2D`
depending on the neural net architecture used for inference. The architecture arguments
specify pre-trained networks. New networks can be trained by the GATK tools: CNNVariantWriteTensors 
and CNNVariantTrain. The CRAM could be generated by the [single-sample pipeline](https://github.com/gatk-workflows/gatk4-data-processing/blob/master/processing-for-variant-discovery-gatk4.wdl).
If you would like test to the workflow on a more representative example file, use the following 
CRAM file as input and change the scatter count from 4 to 200: gs://gatk-best-practices/cnn-h38/NA12878_NA12878_IntraRun_1_SM-G947Y_v1.cram.

#### Requirements/expectations :
 - CRAM/BAM
 - BAM Index (if input is BAM) 

#### Output :
 - Filtered VCF and its index. 

### cram2model.wdl
This **optional** workflow is for advanced users who would like to train a CNN model for filtering variants for specific use cases (e.g. custom panels, non-human, or non-Illumina sequencing). 

#### Requirements/expectations :
 - CRAM
 - Truth VCF and its index
 - Truth Confidence Interval Bed

#### Output :
 - Model HD5
 - Model JSON
 - Model Plots PNG

### run_happy.wdl
This **optional** evaluation and plotting workflow runs a filtering model against truth data (e.g. [NIST Genomes in a Bottle](https://github.com/genome-in-a-bottle/giab_latest_release), [Synthic Diploid Truth Set](https://github.com/lh3/CHM-eval/releases) ) and plots the accuracy.

#### Requirements/expectations :
 - File of VCF Files
 - Truth VCF and its index
 - Truth Confidence Interval Bed

#### Output :
 - Evaluation summary
 - Plots

### Important Notes :
- Runtime parameters are optimized for Broad's Google Cloud Platform implementation.
- For help running workflows on the Google Cloud Platform or locally please
view the following tutorial [(How to) Execute Workflows from the gatk-workflows Git Organization](https://gatk.broadinstitute.org/hc/en-us/articles/360035530952).
- Please visit the [User Guide](https://gatk.broadinstitute.org/hc/en-us/categories/360002310591) site for further documentation on our workflows and tools.

### Contact Us :
- The following material is provided by the Data Science Platforum group at the Broad Institute. Please direct any questions or concerns to one of our forum sites : [GATK](https://gatk.broadinstitute.org/hc/en-us/community/topics) or [Terra](https://support.terra.bio/hc/en-us/community/topics/360000500432).

### LICENSING :
 This script is released under the GATK source code license (Apache 2.0) (see LICENSE in
 https://github.com/broadinstitute/gatk). Note however that the programs it calls may
 be subject to different licenses. Users are responsible for checking that they are
 authorized to run all programs before running this script.
