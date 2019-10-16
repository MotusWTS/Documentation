
# Data flow from receivers

## Receiver types

For the purpose of data flow, there  are currently 2 types of tag encoding (CTT's LifeTags and PowerTags and Lotek's NanoTags) and at least 3 main types of receivers with various configurations.

1. Lotek Receivers: 
- only able to detect NanoTags.
- conversion of tag pulses to tag ID is done by the receiver 
- data files (DTA) are manually recovered and uploaded by users to motus.org
- motusServer and find_tags process the data on sgdata.motus.org and data are transfered into motus.org

2. SensorGnome receivers:
- able to detect NanoTags with FunCube (RTL-SDR also being tested)
- able to detect CTT tags with CTT dongle
- NanoTag and CTT data files (gz archives) can be manually recovered and uploaded by users to motus.org
- NanoTag data can also be sent over SSH to sensorgnome.org, then passed to sgdata.motus.org for processing and finally uploaded by users to motus.org. 
- Not yet clear the status of the new CTT detection file format for connected receivers. **TO CHECK**

3. SensorStation receivers
- able to detect NanoTags 
- able to detect CTT tags 
- both types of data files can be transmitted to CTT over network
- both types of data files can also be manually recovered and uploaded to motus.org

## Possible data paths

**Case 1** : NanoTags on Lotek CVX, with DTA file manually uploaded to motus.org

- File processed by find_tags on sgdata, then imported into motus.org

**Case 2** : NanoTags on SensorGnome, manually uploaded to motus.org

- File processed by find_tags on sgdata, then imported into motus.org

**Case 3** : NanoTags on SensorGnome, sent over SSH to sensorgnome

- File sent by sensorgnome.org to sgdata.motus.org
- File processed by find_tags on sgdata, then imported into motus.org

**Case 4** : NanoTags on SensorStations, sent to CTT over network

- gz files are uploaded by CTT to AWS folder **ctt/data/** 
- gz files are downloaded from AWS by BSC and passed to tag_finder for processing (*S3MonitorProc*)
- gz files are archived on AWS in folder **ctt/processed/**

**Case 5** : CTT tags on SensorGnome (with CTT dongle) or SensorStations, manually uploaded to motus.org 

- gz files are read for known dual mode receivers using the older software to extract CTT detections and GPS data (*ReadCTTDataSG2*)
- gz files containing CTT detections in newer format are intercepted in motusServer and placed in a local folder (*handleNewFiles.R*)
- gz files are sent to AWS drive for CTT processing in folder **motus/data/sg/**  (*UploadCTTDataToS3*) 
- CTT extracts the list of detections matching known tags for Motus users
- filtered files are sent back to AWS drive for BSC in folder **ctt/tag-data** 
- filtered files are read from AWS into motus.org (*ReadCTTFiles*)
- filtered files are archived in AWS in folder **ctt/processed-ctt**

**Case 6** : CTT tags on SensorStations, sent to CTT over network

- gz files are uploaded by CTT to AWS folder **ctt/tag-data/** 
- gz files are downloaded from AWS by BSC and imported into motus.org database (*ReadCTTFiles*)
- gz files are archived on AWS in folder **ctt/processed-ctt/**

