********************************************************************************
* EvaluateSegmentation 1.0
* Evaluation Tool, VISCERAL www.visceral.eu
* Information and Software Engineering Group (IFS)
* Vienna University of Technology
* Abdel Aziz Taha (taha@ifs.tuwien.ac.at)
* Allan Hanbury (hanbury@ifs.tuwien.ac.at)
* 24.06.2012 
********************************************************************************

1 Description
EvaluateSegmentation is a command that compares two volumes (a test segmentation 
and a ground truth segmentation) using 22 different metrics that were selected as a 
result of comprehensive research into the metrics used in the medical volume 
segmentations. EvaluateSegmentation provides the following measures:
 
Similarity
1.	Dice Coefficient
2.	Jaccard Coefficient
3.	Area under ROC Curve (one system state)
4.	Cohen Kappa
5.	Rand Index
6.	Adjusted Rand Index
7.	Interclass Correlation 
8.	Volumetric Similarity Coefficient
9.	Mutual Information
Distance   
10.	Hausdorff Distance
11.	Average Distance
12.	Mahanabolis Distance
13.	Variation of Information
14.	Global Consistency Error
15.	Coefficient of Variation
16.	Probabilistic Distance
Classic measures
17.	Sensitivity (Recall, true positive rate)
18.	Specificity (true negative rate)
19.	Precision 
20.	F-Measure
21.	Accuracy
22.	Fallout (false positive rate)

2 Supported Images 
EvaluateSegmentation supports all 3D file formats that are supported by ITK, 
e.g .nii, .mha, etc. The two Images should however have the same size 
(the same number of voxels). There should be only one label in an image, 
where a voxel value can be either zero (background) or a value between zero and
one [0,1] that denotes the fuzzy membership or the probability that the 
corresponding voxel belongs to the label. 

3 Downloading and Running the tool

3.1 Windows

Download location: http://wiki.visceral.eu/index.php/File:EvaluateSegmentation-1.0-windows.zip

The tool software under windows consists of three files that should be in the 
same directory:
	- EvaluateSegmentation.exe
	- EvaluateSegmentation.exp
	- EvaluateSegmentation.lib

Before the first execution on a Windows machine, a suitable MS redistributable 
should be installed on the machine. This redistributable is a Microsoft tool that 
install the needed dlls. The redistributables can be found here:

http://www.microsoft.com/de-at/download/details.aspx?id=30679
	
Once this is done, the evaluation can be directly performed by invoking the 
command EvaluateSegmentation according to the syntax explained below

The download package also contains two test images truth.nii and segment.nii  

3.2 Linux

Download location: ..............
....
.....

4 Syntax

1) Evaluation of volume segmentations:

USAGE: 

EvaluateSegmentation truthPath segmentPath [-thd threshold] [-xml xmlpath] 
[-use all|DICE,JACRD, ....]

where:
truthPath:	path (or URL) to truth image. URLs should be enclosed with quotations
segmentPath:	path (or URL) to image being evaluated. URLs should be enclosed with quotations
-th threshold:	before evaluation convert fuzzy images to binary using the given
threshold
-xml xmlpath:	path to xml file where results should be saved
-help:		more information	
-use metriclist:this option can be used to specify which metrics should be used. 
Metriclist consists of the codes of the desired metrics separated by commas. 
For those metrics that accept parameters, it is possible to pass these parameters 
by writing them between two @ characters, e.g. –use MUTINF,FMEASR@0.5@. This 
option tells the tool to calculate the mutual information and the F-Measure at beta=0.5.
Possible codes for metriclist are:
		all: calculate all available metrics (default)
		DICE: calculate Dice Coefficient
		JACRD: calculate Jaccard Coefficient
		GCOERR: calculate Global Consistency Error
		VOLSMTY: calculate Volumetric Similarity Coefficient
		KAPPA: calculate Cohen Kappa
		AUC: calculate Area under ROC Curve (one system state)
		RNDIND: calculate Rand Index
		ADJRIND: calculate Adjusted Rand Index 
		ICCORR: calculate Interclass Correlation 
		MUTINF: calculate Mutual Information
		FALLOUT: calculate Fallout (false positive rate)
		COEFVAR: calculate Coefficient of Variation
		AVGDIST: calculate Average Distance
		HDRFDST: calculate Hausdorff Distance HDRFDST@0.95@ means use 
		0.95 quantile to avoid outliers. Default is quantile of 1 which 
		means exact Hausdorff distance
		VARINFO: calculate Variation of Information
		PROBDST: calculate Probabilistic Distance
		MAHLNBS: calculate Mahanabolis Distance
		SNSVTY: calculate Sensitivity (Recall, true positive rate)
		SPCFTY: calculate Specificity (true negative rate)
		PRCISON: calculate Precision 
		FMEASR: calculate F-Measure  FMEASR@0.5@ means use 0.5 as a 
		value for beta in the F-Measure
		ACURCY: calculate Accuracy

-default or -def: Reads default options from a file default.txt in the current folder. All the 
options above except image file names can be used as defaults. Default options are overridden by options given through command line.
		
Examples
Example 1: EvaluateSegmentation truth.nii segment.nii –use RNDIND,HDRFDST@0.96@,
FMEASR@0.5@ -xml result.xml

This example shows how to compare two NIFTI images providing the Rand Index,
Hausdorff distance and the F-Measure and save the results in result.xml. 
The values between two @ symbols are parameters to the specific 
measures in this case the quantile value used with Hausdorff distance to avoid 
outliers. The second value (0.5) is the beta value used with the F-Measure.

Example 2: EvaluateSegmentation truth.nii segment.nii –use all  -th 0.5  

This example compares two images using all available metrics. Before comparing 
the images, they are converted to binary images using a threshold of 0.5, 
that is voxels with values in [0,0.5) are considered as background and those 
with values in [0.5,1] are assigned the label with a membership of 1. 

Example 3: EvaluateSegmentation truth.nii segment.nii –def

This example shows how to use default options (-def). The options should be saved in a file named default.txt.
In this file the options -use, -xml, -th can be saved and they will be used. Option passed to the 
program in addition to -def will override the default option.   

2) Evaluation of landmark localization:

USAGE:
EvaluateSegmentation -loc truth_landmark_path test_landmark_path [-xml output_xml_path]

where:
truth_landmark_path =path (or URL) to file containing ground truth landmarks. URLs should be enclosed with quotations.

test_landmark_path =path (or URL) to file containing landmarks to be evaluated. URLs should be enclosed with quotations.

-xml =path to xml file where result should be saved.

