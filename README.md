# Autism
Code for "🤩".

Part 1: sMRI_git: Using self-defined brain atlas (e.g. volume-based AAL) to extract structual magnetic resonance imaging metrics from surface-based freesurfer.

1. Environment variables that need to be set after installing freesurfer: 
Add the following two lines to the~/. bashrc file:
export FREESURFER_HOME=/histor/sun/linlin/free surfer ## Changing to your freesurfer file path
source $FREESURFER_HOME/SetUpFreeSurfer.sh ## configure freesurfer environment 
then source ~/.bashrc

2. Analysis pipeline: 
(1) Download subjects file from ABIDE: S0_sMRI_1_data_download_git.py
Input files: Phenotypic_V1_0b_preprocessed1.csv, each_sub_necessary_files.txt
Output files: FS_successful_download.json
Note: fsaverage were copied from freesurfer install dictionary: freesurfer/subjects/fsaverage
(2) Construct AAL template: S1_create_AAL_annot.sh
Input files: subject dictionary, *h_aal_olfactory_RGB_note.txt
Output files: *h.aal3.mgh, *h.num${olfactory-related region id}.label, *h_aal_olfactory.annot
(3) Extract structural measurement values based on AAL for each subject: S2_ROI_analysis.sh
Input files:FS_successful_download.json, *h.aal_olfactory.annot, *h.aal_olf_${subjectid}.annot
Output files:${subjectid}_*h.txt
(4) Simplify structural measurement files: S3_AAL_olfactory_extract_simplify.sh
Input files: FS_successful_download.json, ${subjectid}_*h.txt
Output files: ${subjectid}_*h_1.txt
(5) Extract the default measurement values of freesurfer, including total surface area, total gray matter volume, and subcortical gray matter volume: S4_extract_freesurfer_value.sh
Input files: subject dictionary
Output files: FS_git_surface_area.csv, FS_git_gray_matter_volume.csv
(6) Summarize the measurement values of all subjects into one dataframe: S5_statistics_arrange.py
Input files: FS_git_surface_area.csv, FS_git_gray_matter_volume.csv, _*h_1.txt
Output files: FS_git_surface_area_df.csv, FS_git_gray_matter_volume_df.csv

3. Files note: 
(1) AAL brain atlas: AAL116_1mm.nii
(2) The necessary files that each participant needs to download: each_sub_necessary_files.txt
(3) Subject Information File: Phenotypic_V1_0b_preprocessed1.csv
(4) Annotating RGB information of olfactory-related brain regions: lh_aal_olfactory_RGB_note.txt; rh_aal_olfactory_RGB_note.txt
