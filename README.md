# TranSMART-XNAT Connector
This connector consists of components for data capture, organisation and analysis. 

# Features
tranSMART-XNAT Connector consists of components for data capture, organisation and analysis. Data capture is responsible for imaging capture either from PACS system or directly from an MRI scanner, or from raw data files. Data are organised in a similar fashion as tranSMART and are stored in a format that allows direct analysis within tranSMART. 

![](images/1.png?raw=true)
There are many aspects to imaging data which are useful, in addition to the image itself. For example, there is scene data, which records regions of interests highlighted by radiologists. There is also meta data which tells of the software used for pre and postprocessing analytical works, actual analysis measurement results and thumbnails for providing a quick overview of the image volume. 

Image volumes, scene resources and thumbnails will be stored within XNAT. Metadata and analysis results can be stored within tranSMART.

![](images/2.png?raw=true)

The reason we are linking to XNAT is because XNAT is a well tested and tried imaging platform, with extensive support from the imaging community. As imaging requirements grow, the imaging community has taken advantage of the extensibility of XNAT to build plugins to meet their needs. For example, the DICOM C Store connector can be used to send images directly from the imaging device to XNAT. Plugins such as XNAT-Slicer enables images to be stored within imaging software such as 3DSlicer, and for scene data from 3DSlicer to be stored back to XNAT to form accompanying resources for an image.

However, we will leverage tranSMART as a data exploration tool to integrate data types, including information from image analysis. This will enable us to search across data domains, and perform tasks such as the one above “To use a line graph to chart the MSQOL score of subjects, whose received Treatment A, experienced multiple relapses and whose image data show increase in white matter lesions”. 

![](images/3.png?raw=true)

These are the components that we make provision for when designing the tranSMART-XNAT Connector. There are three actions, to caption the image, to organize it, and to analyse it. Images can be captured from PAC systems or in the case of XNAT, it can be received directly from the scanner. XNAT has the provisions for organizing, viewing and downloading images. An image analysis software like 3D Slicer provides tools for measuring and recording areas of interest. Data is also captured from spreadsheets, and curated and organized in a platform like tranSMART.

![](images/4.png?raw=true)
There are multiple ways or pulling images from PAC systems and scanners. The choice of method depends of the task at hand. Images can be sent daily automatically from scanners. Scripts can be written to take advantage of the XNAT API to upload images in large batches. Finally, there are applets that allows selection of individual images from computers. Clinical data, high dimensional data, and image-related results can be stored into tranSMART 1.2 using tranSMART data.

# Prerequisite 

It is essential to set up tranSMART before deploy the tranSMART-XNAT Connector.

For installation of tranSMART, please visit [here](https://wiki.transmartfoundation.org/display/TSMTGPL/tranSMART+1.2+INSTALLATION+NOTES+ON+UBUNTU) for more detail. 

# Installation of tranSMART-XNAT Connector

The following shows the installation procedure for tranSMART-XNAT Connector

Please download the code using Git to a local directory.

Add the following in “grails-app/conf/BuildConfig.groovy”.

````
    grails.plugin.location.'transmart-xnat' = "[Your xnat directory]"
````

Change the following in “grails-app/conf/Config.groovy” if you would like to connect to your own XNAT server, example:
````
    org.xnat.domain = 'central.xnat.org'
    org.xnat.username = 'your xnat username'
    org.xnat.password = 'your xnat password'
```` 
Import the database schema to the same PostgreSQL database as tranSMART.
````
    CREATE SCHEMA xnat
      AUTHORIZATION biomart_user;

    CREATE TABLE xnat.subject
    (
      tsmart_subjectid character varying(20),
      xnat_subjectid character varying(20),
      xnat_project character varying(80),
      id serial NOT NULL,
      CONSTRAINT subject_pkey PRIMARY KEY (id)
    )
    WITH (
      OIDS=FALSE
    );
    ALTER TABLE xnat.subject OWNER TO biomart_user;
````
After the scheme is generated, please fill in the xnat.subject table as shown below.

| tsmart_subjectid | xnat_subjectid | xnat_project | id |
| --- | --- | --- | --- |
| OPT_001 | OPT_001 | eTRIKS | 1 |
| OPT_002 | OPT_002 | eTRIKS | 2 |

* tsmart_subjectid denotes the id of the subject in tranSMART. 

* xnat_subjectid denotes the id of the corresponding subject in XNAT. Each row of IDs refer to the same subject, in this example above, the subject IDs are the same for tranSMART and XNAT. If they differ, enter the id names appropriately. 

* xnat_project is the id of the project in XNAT which contains the subjects.

* id is self-defined unique number to identify the subject within the table.

Start the tranSMART, the plugin will be loaded automatically.

# Contributing

We encourage you to contribute to the plugin if you find any issues or missing
functionality that you'd like to see. 

# License

The plugin is distributed under Apache License, version 2.0.
