![SAP HANA Academy](https://yt3.ggpht.com/-BHsLGUIJDb0/AAAAAAAAAAI/AAAAAAAAAVo/6_d1oarRr8g/s100-mo-c-c0xffffffff-rj-k-no/photo.jpg)

## SAP HANA on Google Cloud Platform ##
In the first video, we discuss planning and preparation. We assume you already have signed up for Google Cloud Platform, hvae created a project, enabled billing, etc. If not, please check the documentation below, SAP HANA Planning Guide. 

https://cloud.google.com/solutions/images/sap-hana-deployment.svg

### Tutorial Video ### 
[![SAP HANA on Google Cloud Platform](https://img.youtube.com/vi/W62V7jZQQPI/0.jpg)](https://www.youtube.com/watch?v=W62V7jZQQPI "SAP HANA on Google Cloud Platform")

### Documentation ### 
* [SAP HANA Planning Guide](https://cloud.google.com/solutions/partners/sap/sap-hana-planning-guide)
* [All SAP HANA Guides](https://cloud.google.com/solutions/partners/sap/sap-hana-guides)
* [Google Cloud Console](https://console.cloud.google.com/)
* [Installing Google Cloud SDK](https://cloud.google.com/sdk/install)

## Create Cloud Storage Bucket ##
To automate SAP HANA deployments on the Google Cloud Platform, you need to download the installation media from SAP Downloads (software download center) and upload the set to a Google Cloud Storage (GCS) bucket. In this video tutorial, we show how you can do this. 

You can only download the SAP products that are associated with your S-user ID. For more information on this topic, see [Everything you need to know about S-User ID](https://www.sapanalytics.cloud/resources-s-user-id/).  

If you would like to learn more about downloading SAP software, see Software Download tutorial video from the SAP HANA platform installation and update playlist. 

### Tutorial Video ### 
[![Create Cloud Storage Bucket](https://img.youtube.com/vi/CV4CaBbI06o/0.jpg)](https://www.youtube.com/watch?v=CV4CaBbI06o "Create Cloud Storage Bucket")

### Documentation ### 
* [Creating Storage Buckets](https://cloud.google.com/storage/docs/creating-buckets#storage-create-bucket-console)
* [SAP HANA PLATFORM EDITION 2.0 (INSTALLATIONS AND UPGRADES)](https://launchpad.support.sap.com/#/softwarecenter/template/products/related/_APP=00200682500000001943&_EVENT=DISPHIER&HEADER=Y&FUNCTIONBAR=N&EVENT=TREE&NE=NAVIGATE&ENR=73554900100900001301&V=INST/SAP%20HANA%20PLATFORM%20EDITION%202.0%20(SUPPORT%20PACKAGES%20AND%20PATCHES))
* [Everything you need to know about S-User ID](https://www.sapanalytics.cloud/resources-s-user-id/)
* [SAP HANA Deployment Guide](https://cloud.google.com/solutions/partners/sap/sap-hana-deployment-guide)
* [SAP HANA Academy - SAP HANA Installation and Update: Software Download [2.0 SPS 02]](https://www.youtube.com/watch?v=kPCSa7Z4qH8&list=PLkzo92owKnVxLSwL08JT7TwqcynRJcRoe&index=26)

## Deployment Configuration ##
The Google Cloud Platform Deployment Manager takes as imput a configuration file (https://storage.googleapis.com/sapdeploy/dm-templates/sap_hana/template.yaml). In this video tutorial, we explain the different parameters. 

If you are new to the topic of SAP HANA server installations, you might find the video tutorial Server Installation of interest, where we show what kind of configuration decisions usually need to be made. 
 
```
    instanceName: [VM_NAME]
    instanceType: [MACHINE_TYPE]
    zone: [ZONE]
    subnetwork: [SUBNETWORK]
    linuxImage: [IMAGE_FAMILY]
    linuxImageProject: [IMAGE_PROJECT]
    sap_hana_deployment_bucket: [MEDIA_BUCKET]
    sap_hana_sid: [SID]
    sap_hana_instance_number: [INSTANCE_NUMBER]
    sap_hana_sidadm_password: [PASSWORD]
    sap_hana_system_password: [PASSWORD]
    sap_hana_scaleout_nodes: [NUMBER_OF_WORKER_NODES]
 ```

### Tutorial Video ### 
[![Deployment Configuration](https://img.youtube.com/vi/hz0pDEsOvuo/0.jpg)](https://www.youtube.com/watch?v=hz0pDEsOvuo "Deployment Configuration")

### Documentation ### 
* [SAP HANA Academy - SAP HANA Installation and Update: Server Installation[2.0 SPS 02]](https://www.youtube.com/watch?v=NNWGg9u9cw4&list=PLkzo92owKnVxLSwL08JT7TwqcynRJcRoe&index=20)

## Single-Node Deployment ##
In this tutorial, we perform an simple deployment of a single-node SAP HANA system on the default network. Consider this as a dry run just to verify we can access the GCS bucket, we have the prequired project privileges to create GCE instances, etc. 

For production environments it would be prudent to run the SAP HANA system on a separate subnet and only enable access from a bastion host (see below). 

```
gcloud deployment-manager deployments create [DEPLOYMENT-NAME] --config [TEMPLATE-NAME].yaml
```

### Tutorial Video ### 
[![Single-Node Deployment](https://img.youtube.com/vi/lP7vEI9maEE/0.jpg)](https://www.youtube.com/watch?v=lP7vEI9maEE "Single-Node Deployment")

### Documentation ### 
* [SAP HANA Deployment Guide](https://cloud.google.com/solutions/partners/sap/sap-hana-deployment-guide)

## Single-Node Deployment (Under the Hood) ##
In this video, we will take a look at the nuts and bolts a bit, take a look “under the hood” so to speak, as this may help to locate issues should anything go wrong with the deployment or produce results that were not expected. 

Files examined include: 
* the GCE instance startup file (https://storage.googleapis.com/sapdeploy/dm-templates/sap_hana/sap_hana.py) 
* bash script file(s) (https://storage.googleapis.com/sapdeploy/include/sap_func_hdb.sh)

```
hdb-install() {
	main-errhandle_log_info 'Installing SAP HANA'
	/hana/shared/media/51*/DATA_UNITS/HDB_LCM_LINUX_X86_64/hdblcm --configfile=/root/.deploy/${HOSTNAME}_hana_install.cfg -b

	## check extraction worked
	if  [ ! $? -eq 0 ]; then
		main-errhandle_log_error "HANA Installation Failed. The server deployment is complete but SAP HANA is not deployed. Manual SAP HANA installation will be required"
	fi
}
```

### Tutorial Video ### 
[![Single-Node Deployment](https://img.youtube.com/vi/5zZrc6gLMX8/0.jpg)](https://www.youtube.com/watch?v=5zZrc6gLMX8 "Single-Node Deployment (Under the Hood)")

### Documentation ### 
* [SAP HANA Deployment Guide](https://cloud.google.com/solutions/partners/sap/sap-hana-deployment-guide)
