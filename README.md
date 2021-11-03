# TtoZ

[![DOI](https://zenodo.org/badge/doi/10.5281/zenodo.32508.svg)](http://dx.doi.org/10.5281/zenodo.32508)

TtoZ is a python implementation of Hughett's t-to-z tranform for whole brain statistical maps. 

For background about the problem, see [Hughett's paper](doc/JStats_Hughett.pdf). For a detailed example of the problem pertaining to whole brain statistical maps, see [this ipython notebook](http://nbviewer.ipython.org/github/vsoch/TtoZ/blob/master/doc/t_to_z_procedure.ipynb). For a more concise summary of the particular problems addressed by Hughett's algorithm, as compared to traditional procedures using scipy see [this ipython notebook](http://nbviewer.ipython.org/github/vsoch/TtoZ/blob/master/doc/TtoZ_method_comparison.ipynb).

[![distributions](doc/histograms.png)](https://raw.githubusercontent.com/vsoch/TtoZ/master/doc/histograms.png)  

Here we show the image histograms (from mricron) for: 

 1. [the original t-stat map](example/tfMRI_LANGUAGE_STORY.nii_tstat1.nii.gz) with 484 degrees of freedom, 
 2. a [z-score map](example/zstat_afni.nii) produced with AFNI's 3dcalc (`3dcalc -a tstat.nii.gz -expr 'fitt_t2z (a,484)' -prefix afni_zstat.nii -float`), 
 3. a [z-score map](example/zstat_nosplit.nii) [our original effort](http://nbviewer.ipython.org/github/vsoch/TtoZ/blob/master/doc/t_to_z_procedure.ipynb#Converting-from-P-Values-to-Z-Scores) using scipy.stats that did not split into +/- values, and 
 4. a [z-score map](example/zstat_ttz.nii) produced with TtoZ (Hughett's method that splits at 0 into two sets first). 

2) and 3) have truncation of the distributions.  For both 3) and 4) it was also necessary to convert p-values exactly == 1 to 0.999999, as a value of exactly 1 will be converted to inf when converted to the standard normal score.

Note that the y-axis scales are slightly different, most noticably for the second image, 3dcalc, which extends to a density of 2.0 due to the number of zeros that are replaced to prevent error. 

# Installation

     mamba create --name t2z python=3.6.13 pip=21.3.1 seaborn=0.11.2 numpy=1.19.5 nibabel=3.2.1 nilearn=0.8.1 matplotlib=3.3.4 jupyter=1.0.0

     conda activate t2z
     
     pip install TtoZ


# Usage

      usage: TtoZ [-h] --t_stat_map T_STAT_MAP --dof DOF [--output_nii OUTPUT_NII]

      Convert a whole brain T score map to a Z score map without loss of precision
      for strongly positive and negative values.

      optional arguments:
       -h, --help            show this help message and exit
        --t_stat_map T_STAT_MAP
                              T-score statistical map in the form of a 3D NIFTI file
                              (.nii or .nii.gz).
        --dof DOF             Degrees of freedom (eg. for a two-sample T-test:
                              number of subjects in group - 2)
        --output_nii OUTPUT_NII
                              The name for the output Z-Score Map.


# Example

     TtoZ --t_stat_map=t_stat_map.nii.gz --dof=484 --output_nii=z_score_map.nii

