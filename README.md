# Autism
Code for "🤩".

Part 1: sMRI_git: Using self-defined brain atlas (e.g. volume-based AAL) to extract structual magnetic resonance imaging metrics from surface-based FreeSurfer.

Prerequisites
Before starting, ensure that FreeSurfer is properly installed and set up in your environment. The following steps will guide you through configuring the necessary environment variables.
1. Setting Up FreeSurfer
After installing FreeSurfer, set up the environment by adding the following lines to your ~/.bashrc file:
export FREESURFER_HOME=/path/to/your/freesurfer  # Modify this to your FreeSurfer installation directory
source $FREESURFER_HOME/SetUpFreeSurfer.sh       # Source the FreeSurfer setup script
Then, apply the changes by running:
source ~/.bashrc

3. Analysis pipeline:
The pipeline consists of several steps to preprocess, analyze, and summarize structural MRI data using FreeSurfer and a self-defined AAL brain atlas.
(1) Download Subject Files from ABIDE
· Script:
S0_sMRI_1_data_download_git.py
· Input Files:
Phenotypic_V1_0b_preprocessed1.csv
each_sub_necessary_files.txt
· Output Files:
FS_successful_download.json
· Notes:
fsaverage template is copied from the FreeSurfer installation directory: $FREESURFER_HOME/subjects/fsaverage.
(2) Construct AAL Template
· Script:
S1_create_AAL_annot.sh
· Input Files:
Subject dictionary
*h_aal_olfactory_RGB_note.txt
· Output Files:
*h.aal3.mgh
*h.num{olfactory-related region id}.label
*h_aal_olfactory.annot
(3)  Extract Structural Measurement Values Based on AAL for Each Subject
· Script:
S2_ROI_analysis.sh
· Input Files:
FS_successful_download.json
*h.aal_olfactory.annot
*h.aal_olf_{subjectid}.annot
· Output Files:
${subjectid}*h.txt
(4) Simplify Structural Measurement Files
· Script:
S3_AAL_olfactory_extract_simplify.sh
· Input Files:
FS_successful_download.json
${subjectid}*h.txt
· Output Files:
${subjectid}_*h_1.txt
(5) Extract Default Measurement Values from FreeSurfer
· Script:
S4_extract_freesurfer_value.sh
· Input Files:
Subject dictionary
· Output Files:
FS_git_surface_area.csv
FS_git_gray_matter_volume.csv
(6) Summarize the Measurement Values for All Subjects into One DataFrame
· Script:
S5_statistics_arrange.py
Input Files:
FS_git_surface_area.csv
FS_git_gray_matter_volume.csv
_ *h_1.txt
· Output Files:
FS_git_surface_area_df.csv
FS_git_gray_matter_volume_df.csv

File Notes
(1) AAL Brain Atlas:
AAL116_1mm.nii
(2) Necessary Files for Each Participant:
each_sub_necessary_files.txt
(3) Subject Information File:
Phenotypic_V1_0b_preprocessed1.csv
(4) Annotating RGB Information for Olfactory-Related Brain Regions:
lh_aal_olfactory_RGB_note.txt
rh_aal_olfactory_RGB_note.txt
