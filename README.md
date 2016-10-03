# TranSMART-XNAT Connector
This connector consists of components for data capture, organisation and analysis. 

# Features
tranSMART-XNAT Connector consists of components for data capture, organisation and analysis. Data capture is responsible for imaging capture either from PACS system or directly from an MRI scanner, or from raw data files. Data are organised in a similar fashion as tranSMART and are stored in a format that allows direct analysis within tranSMART. 



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
