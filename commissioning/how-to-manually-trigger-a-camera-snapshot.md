# How to manually trigger a camera snapshot

![](../.gitbook/assets/0.png)

How to manually trigger a camera snapshot

v.2024.05.22

**Overview:**

This document will describe how to trigger a photograph on an IP camera connected to an Econode

**Tools and Information required:**

* Windows or Linux Laptop with Chrome, Firefox or Safari browser and SSH command line
* Credentials to the Econode you are modifying

**Step by Step procedure**

**Step 1.** SolarSSH to the node

**Step 2.** User SolarNetwork API Explorer to execute a snapshot

/solaruser/api/v1/sec/instr/add/Signal

{"nodeId":560, "params":{"/camera/1":"snapshot"\}}

**Step 3.** Watch the log file to see that the photo is taken

2022-01-30 14:55:44 INFO MqttUploadService; Instruction 11558850 Signal received with parameters: {/camera/1=snapshot}

2022-01-30 14:55:44 INFO MqttUploadService; Posted Instruction 11558850 \[Signal] acknowledgement status: Executing

2022-01-30 14:55:45 INFO MqttUploadService; Uploaded datum via MQTT: Datum{kind=Node,sourceId=/SE/ST/S1/GEN/1,ts=2022-01-30T19:55:45.011Z,data={i={frequency=60.02, voltage=77.24, lineVoltage=154.47, current=119.58, powerFactor=0.9711, apparentPower=13870, reactivePower=3102, watts=13460, current\_a=59.6, voltage\_a=116.03, voltage\_ab=231.71, current\_b=59.98, voltage\_b=115.69, voltage\_bc=115.69, current\_c=0.0, voltage\_c=0.0, voltage\_ca=116.03}, a={wattHours=236389900, wattHoursReverse=484200}, s={phase=Total\}}}

2022-01-30 14:55:47 INFO ResourceStorageServiceDirectoryWatcher; Delaying save of watched file /var/lib/solarnode/var/ffmpeg-images/ffmpeg-20220130T195544Z-snapshot.jpg for 10000ms

2022-01-30 14:55:47 INFO ResourceStorageServiceDirectoryWatcher; Delaying save of watched file /var/lib/solarnode/var/ffmpeg-images/ffmpeg-20220130T195544Z-snapshot.jpg for 10000ms

2022-01-30 14:55:47 INFO FfmpegCameraControl; Snapshot successful for ffmpeg snapshot /camera/1: var/ffmpeg-images/ffmpeg-20220130T195544Z-snapshot.jpg

2022-01-30 14:55:47 INFO SimpleInstructionExecutionService; Instruction 11558850 \[Signal] state changed to Completed

2022-01-30 14:55:47 INFO MqttUploadService; Posted Instruction 11558850 \[Signal] acknowledgement status: Completed

2022-01-30 14:55:48 WARN DatumDataSourcePollManagedJob; Communication problem collecting data from net.solarnetwork.node.datum.yaskawa.pvi3800.PVI3800DatumDataSource@1ea0ae6: net.solarnetwork.node.service.LockTimeoutException: Could not acquire port /dev/ttyUSB\_4 lock

2022-01-30 14:55:57 INFO ResourceStorageServiceDirectoryWatcher; Saving resource /var/lib/solarnode/var/ffmpeg-images/ffmpeg-20220130T195544Z-snapshot.jpg to NodeS3ResourceStorageService{uid=Camera Storage,client=S3Client{region=us-west-2,bucket=ecogy-solarnodes},path=solarnode-resource-upload/353/}

2022-01-30 14:55:57 INFO ResourceStorageServiceDirectoryWatcher; 31.9% complete saving resource ffmpeg-20220130T195544Z-snapshot.jpg to NodeS3ResourceStorageService{uid=Camera Storage,client=S3Client{region=us-west-2,bucket=ecogy-solarnodes},path=solarnode-resource-upload/353/}

2022-01-30 14:55:57 INFO ResourceStorageServiceDirectoryWatcher; 63.8% complete saving resource ffmpeg-20220130T195544Z-snapshot.jpg to NodeS3ResourceStorageService{uid=Camera Storage,client=S3Client{region=us-west-2,bucket=ecogy-solarnodes},path=solarnode-resource-upload/353/}

2022-01-30 14:55:57 INFO ResourceStorageServiceDirectoryWatcher; 95.6% complete saving resource ffmpeg-20220130T195544Z-snapshot.jpg to NodeS3ResourceStorageService{uid=Camera Storage,client=S3Client{region=us-west-2,bucket=ecogy-solarnodes},path=solarnode-resource-upload/353/}

2022-01-30 14:55:58 INFO ResourceStorageServiceDirectoryWatcher; 100.0% complete saving resource ffmpeg-20220130T195544Z-snapshot.jpg to NodeS3ResourceStorageService{uid=Camera Storage,client=S3Client{region=us-west-2,bucket=ecogy-solarnodes},path=solarnode-resource-upload/353/}

2022-01-30 14:55:58 INFO ResourceStorageServiceDirectoryWatcher; Resource /var/lib/solarnode/var/ffmpeg-images/ffmpeg-20220130T195544Z-snapshot.jpg saved to NodeS3ResourceStorageService{uid=Camera Storage,client=S3Client{region=us-west-2,bucket=ecogy-solarnodes},path=solarnode-resource-upload/353/}.

Additionally you will see an image that was just taken if you shortly return to the Plugin configuration page.

**Snapshot Options:**

**0 0 8,12,16 \* \* \* for 8am, noon and 4pm photos**

If the snapshot options are not configured properly you will not see an image appear on the plug configuration page. These are the cameras we have used:

For Amcrest 5MP:

-rtsp\_transport tcp -i rtsp://admin:SolarSilo23$@192.168.6.230:554/cam/realmonitor?channel=1\&subtype=0 -vframes 1 -vf scale=480:-1 -q:v 2

-rtsp\_transport tcp -i rtsp://admin:admin$@192.168.6.119:554/cam/realmonitor?channel=1\&subtype=0 -vframes 1 -vf scale=480:-1 -q:v 2

**ffmpeg commandline:**

/usr/bin/ffmpeg -rtsp\_transport tcp -i rtsp://admin:SolarSilo23$@192.168.6.230:554/cam/realmonitor?channel=1\&subtype=0 -vframes 1 -vf scale=480:-1 -q:v 2

/usr/bin/ffmpeg -rtsp\_transport tcp -i rtsp://admin:SolarSilo23$@192.168.6.114:554/cam/realmonitor?channel=1\&subtype=0 -vframes 1 -vf scale=480:-1 -q:v 2

/usr/bin/ffmpeg -rtsp\_transport tcp -i rtsp://admin:admin@192.168.6.119:554/cam/realmonitor?channel=1\&subtype=0 -vframes 1 -vf scale=480:-1 -q:v 2

For Ubiquity:

-rtsp\_transport tcp -i rtsp://ubnt:ubnt@192.168.6.230:554/s0 -vframes 1 -vf scale=480:-1 -q:v 2

/usr/bin/ffmpeg -rtsp\_transport tcp -i rtsp://ubnt:ubnt@192.168.6.230:554/s0 -vframes 1 -vf scale=480:-1 -q:v 2

/usr/bin/ffmpeg -rtsp\_transport tcp -i rtsp://ubnt:ubnt@192.168.178.140:554/s0 -vframes 1 -vf scale=480:-1 -q:v 2 /var/lib/solarnode/var/ffmpeg-images/ffmpegtest1-20240618T035000Z-snapshot.jpg

For SCW 2MP camera:

-rtsp\_transport tcp -i rtsp://admin:SolarSilo23@192.168.3.50:554/s0 -vframes 1 -vf scale=480:-1 -q:v 2

![](<../.gitbook/assets/1 (4).png>) ![](<../.gitbook/assets/2 (3).png>)
