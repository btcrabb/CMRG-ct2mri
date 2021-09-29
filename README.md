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

### 3.1 Prescribe cardiac planes

The views necessary for CIM modeling are 4-chamber, short-axis, left ventricular outflow tract (LVOT) or 3-chamber, right ventricular outflow tract (RVOT), 2-chamber left, 2-chamber right. Use the 3D MPR tool on Osirix to generate each of these views. 

### 3.2 Export DICOMs

Use the following settings to export the long- and short-axis images. 

## Step 4: Format DICOMs to be CIM compatible

Use the notebook ct2mri.ipynb to change the DICOMs exported from Osirix into a CIM compatible format. 
