CT2MRI - Generate CIM Compatible DICOMs
==============================
Author: Brendan Crabb

Email: brendan.crabb@hsc.utah.edu


Project Organization
------------

    ├── data
    │   ├── dicoms         <- Original DICOM headers
    │   ├── nii_files      <- Original NII files.
    │   ├── volumes        <- Dicoms generated from the NII files.
    │   ├── osirix         <- Dicoms (by cardiac view) generated in Osirix
    │   └── CIM ready      <- Dicoms ready for CIM
    |
    ├── nii2dcm.ipynb      <- Notebook for generating DICOMs from nii files
    │
    └── ct2mri.ipynb       <- Notebook for converting Osirix export into CIM compatible DICOM files
                              
--------

Directions and python scripts for generating CIM-compatible DICOM files from CT images for statistical cardiac modeling.


## Getting Started

These directions lay out step by step instructions for generating CIM compatible DICOM files from CT images. 

## Step 1: Download DICOM and NII files for each patient

The directory structure originally used with these scripts is shown below. It is suggested that you format your directories accordingly; however, different input directories can be specified in the corresponding jupyter notebooks.

------------

    └── data
        ├── dicoms               
        |      └── patientID
        |              └── SeriesID
        |                     ├── image1.dcm
        |                     ├── image2.dcm
        |                     └── ...
        └── nii_files                    
               └── patientID
                        ├── 1.nii.gz
                        ├── 2.nii.gz
                        └── ...
                              
--------

## Step 2: Generate Complete Volumes from NII Files

Using the notebook nii2dcm.ipynb, generate dicom files corresponding to the complete CT axial volume. The generated DICOM files are compatible with Osirix, which can be used to perform 3D multiplanar reconstruction (MPR) and prescribe the appropriate cardiac MRI planes necessary for cardiac modeling in CIM. 

Once the DICOMs are generated, load the files into Osirix as specified below.

## Step 3: Prescribe Cardiac Planes using Osirix

* Note - If you are currently using Windows, you will be tempted to try a windows-compatible dicom viewer to perform MPR and save the necessary views. Don't do it. It is not worth it. Don't even think about virtual machines, EC2 instances running MacOS, etc. Speaking from experience, the easiest route is to simply find an available Mac and use Osirix/horos. 

### 3.1 3D MPR

Open the DICOM files generated by the nii2dcm.ipynb notebook using Osirix. Once loaded, there is an option to run 3D MPR as shown below: 

![Alt text](figures/3dmpr.png?raw=true "3D MPR Option in Osirix")

Next, use the 3D MPR tool to generate the cardiac views. The views necessary for CIM modeling are 4-chamber, short-axis, left ventricular outflow tract (LVOT) or 3-chamber, and right ventricular outflow tract (RVOT). The specific steps to prescribing each of these planes can be found online. I found the this guide published by the AAMC helpful: https://www.mededportal.org/doi/10.15766/mep_2374-8265.9906

![Alt text](figures/planes.png?raw=true "Prescribing Cardiac Planes")

For long axis images (4-chamber, LVOT, RVOT, etc) use the following settings when exporting:

#### Important Settings:
* Size: 512x512 (larger images will cause a memory error in CIM)
* Include all 4D series - Check box
* Series name: Give the series the appropriate name

![Alt text](figures/la_settings.png?raw=true "Long-Axis Settings")

For short-axis, use the "Batch" feature to select multiple slices through the ventricles. For CIM, having 8-10 slices ranging from the apex to the base is typically sufficient. 

![Alt text](figures/sa_settings.png?raw=true "Short-Axis Settings")

### 3.2 Export DICOMs

Now that each of the views has been generated, export the files as DICOMs. Long-axis and short-axis views should be exported separately for use with the ct2mri.ipynb notebook in the next step. Importantly, select a Hierarchical folder tree, with folder names for the patient's name, study ID, and series name as shown below. 

![Alt text](figures/export.png?raw=true "Export Settings")

## Step 4: Format DICOMs to be CIM compatible

Use the notebook ct2mri.ipynb to change the DICOMs exported from Osirix into a CIM compatible format. This notebook reformats the DICOMs to be organized by time, instead of by location in space. Furthermore, it adds necessary DICOM header tags (such as trigger time, R-R interval, etc). After this step, the images may be loaded into CIM! 
