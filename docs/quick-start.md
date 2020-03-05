
# Quick Start Guide: Rubrik Add-On for Splunk

## Introduction to the Rubrik Add-On for Splunk

Rubrik’s [API first architecture](https://www.rubrik.com/product/api-integration/) allows for integration with a wide array of monitoring and visibility tools. This quick start guide will provide everything needed to begin streaming metrics from Rubrik CDM and Polaris into Splunk, allowing you to consume dashboards, alerts, and analytics in an easy to use web interface. Data is ingested by Splunk via REST API and analyzed by the Splunk platform, presenting the following information:

* Event data - security, replication, backup, recovery, archive and more
* Capacity statistics and trending
* Backup and recovery histories and trending
* Ransomware detection events via [Polaris Radar](https://www.rubrik.com/product/polaris-radar/)

The Rubrik Add-On for Splunk is comprised of two pieces: the Rubrik
Splunk Add-On, available on
[Splunkbase](https://splunkbase.splunk.com/app/4119/),
and the Rubrik Splunk Application, also available on
[Splunkbase](https://splunkbase.splunk.com/app/4570/). The
add-on performs the heavy lifting of communicating with the Rubrik REST
API endpoint to gather metrics, while the application contains the
dashboards and other logic used for visualization in Splunk.

## Installing the Rubrik Add-On for Splunk

This section details the steps required to install all necessary
components for the Rubrik Add-On for Splunk.

### Prerequisites

* Splunk 7.0 or greater
* [Splunk Datasets Add-On](https://splunkbase.splunk.com/app/3245/)

### Installing the Rubrik Splunk Add-On

Search for the Rubrik Splunk Add-On by clicking on **Apps** → **Find
More Apps** in your Splunk GUI, or browse to
`http://<Your Splunk Server>:8000/en-US/manager/system/appsremote`.
Perform a search for “Rubrik”, and the Rubrik Splunk Add-On will be
displayed in the results. Click ‘Install’, and complete the displayed
dialog.

![](https://user-images.githubusercontent.com/12414122/51684170-84dd0980-1fb9-11e9-9b0a-02bfd2626396.png)

After installation is complete, you will be prompted to restart your
Splunk server. Click **Restart Now** or **Restart Later** depending on
your preference. Continue with the installation steps once the restart
is complete.

After logging back into the Splunk server, you will see the **Rubrik
Splunk Add-On** listed in the Apps menu.

### Installing the Rubrik Splunk Application

Search for the Rubrik Splunk Add-On by clicking on **Apps** → **Find
More Apps** in your Splunk GUI, or browse to
`http://<Your Splunk Server>:8000/en-US/manager/system/appsremote`.
Perform a search for “Rubrik”, and the Rubrik Splunk Application will be
displayed in the results. Click ‘Install’, and complete the displayed
dialog.

After installation is complete, you will be prompted to restart your
Splunk server. Click **Restart Now** or **Restart Later** depending on
your preference. Continue with the installation steps once the restart
is complete.

After logging back into the Splunk server, you will see the **Rubrik Splunk Application** listed in the Apps menu.

## Configuration

This section details the steps required to configure all necessary
components for the Rubrik Add-On for Splunk.

### Configure Monitoring for Cloud Data Management

The next step is to configure credentials for your Rubrik CDM
cluster(s). Click on **Apps** → **Rubrik Splunk Add-On**. Click on
**Configuration**, then click **Add**.

![](https://user-images.githubusercontent.com/12414122/51684174-84dd0980-1fb9-11e9-8641-0e9ca0e95013.png)

This will display the **Add Account** dialog. Provide an account name,
username and password for Splunk to connect to your CDM cluster via REST
API. Repeat this step for each cluster you would like to monitor. The
value you provide for **Account Name** will be used to identify each
cluster in dashboards. As an example, if you are monitoring two
clusters, you could use `Cluster_A` and `Cluster_B` for the account
names.

| Note: Account name must start with a letter and be followed by alphanumeric characters or underscores. |
| --- |

After adding all necessary accounts, click on the **Logging** tab and
verify that the log level is set to provide the desired amount of
detail. **INFO** is the default selection, and should be adequate for
most use cases.

#### Creating Inputs

Inputs specify data that will be gathered from the Rubrik REST API
endpoint. You will create four new inputs via the Splunk GUI. From
within the Rubrik Splunk Add-On, click **Inputs**, then **Create New
Input**. The dropdown will display a list of all possible input types.
Create inputs based on the values
below.

![](https://user-images.githubusercontent.com/12414122/51684175-84dd0980-1fb9-11e9-8afe-6336bb74fe46.png)

| Note:​ If you are adding multiple Rubrik clusters, then it is a good idea to include a short version of the cluster name in the `Name` field, in this case, replace rubrik with the short name of your cluster. |
| --- |

| Note:​ It is a good idea to use a floating IP address for the `Rubrik Node` value. This will ensure that in the case of a node being unavailable, the data points can still be gathered. Instructions on setting up floating IPs can be found in the Rubrik User Guide, which is available on the Rubrik Support Portal. |
| --- |

| Note:​ For each input you are able to choose to validate SSL certificates for the Rubrik cluster. This is disabled by default, but the 'Verify SSL Certificate' box can be checked on each input to enable this. |
| --- |

| **Input Type**     | Rubrik - Runway remaining                    |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_runway\_remaining                    |
| **Interval**       | 3600                                         |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Storage Summary                     |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_storage\_summary                     |
| **Interval**       | 600                                          |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Cluster IO Stats                    |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_cluster\_io\_stats                   |
| **Interval**       | 60                                           |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Event Feed                          |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_event\_feed                          |
| **Interval**       | 300                                          |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Archive Location Bandwidth          |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_archive\_location\_bandwidth         |
| **Interval**       | 840                                          |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Archive Location Usage              |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_archive\_location\_usage             |
| **Interval**       | 3600                                         |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Organization Capacity Report        |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_org\_capacity\_report                |
| **Interval**       | 1800                                         |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Managed Volume Summary              |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_mv\_summary                          |
| **Interval**       | 600                                          |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Node IO Stats                       |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_node\_io\_stats                      |
| **Interval**       | 60                                           |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

| **Input Type**     | Rubrik - Node Stats                          |
| ------------------ | -------------------------------------------- |
| **Name**           | rubrik\_node\_stats                          |
| **Interval**       | 60                                           |
| **Index**          | main                                         |
| **Global Account** | \<Account Name defined in previous section\> |
| **Rubrik Node**    | \<FQDN or floating IP\>                      |

Below is an example of what your **Inputs** screen would look like if
you have two clusters monitored by Splunk.

![](https://user-images.githubusercontent.com/12414122/51684176-8575a000-1fb9-11e9-937e-e91054b5db88.png)

There is also a custom report input which can be used to pull in data from a Rubrik Envision
custom report, when adding this we need the custom report ID which is available in the URL of the
UI when viewing the report, an example of a custom report ID format is: `CustomReport:::63db2599-54dc-490b-b199-5f54cfd231c7`.
It is suggested that when adding this input, an interval of 21600 (6 hours) is used, but as with the
other inputs, this can be tweaked as required. Datasets for custom report inputs can be created in a similar
way as below, but the required fields will vary depending on the report, so instructions for this are not
included below.

#### Creating Datasets

The [Splunk Datasets
Add-On](https://splunkbase.splunk.com/app/3245/) can be used to
create new tables from gathered data. In this scenario, five new
datasets will be created to be used by the predefined dashboards.
Install this add-on before continuing if it is not already present.

You will use the search strings below to create each dataset. To create
a new dataset, click on **Apps** → **Rubrik**, then **Create New Table
Dataset**.

![](https://user-images.githubusercontent.com/12414122/51684177-8575a000-1fb9-11e9-9e08-0d83a8d1f866.png)

Click on **Search (Advanced)**, paste in the search string, and click
the green search button. This will display a list of available fields
along with search results. Select the fields specified with the search
string and click **Done**. Note that some fields, like **\_time** and
**\_raw**, may be auto-selected for you. Click **Done**, and a preview
of the dataset will be displayed. Click **Save As**, input the table
title and table ID, and click **Save**. Below is an example of creating
the **Rubrik - Runway Remaining** dataset.

1\. Perform a search using the Search String, select the desired fields,
and click **Done**.

![](https://user-images.githubusercontent.com/12414122/51684179-8575a000-1fb9-11e9-8b5d-e07b7a61fcb1.png)

2\. Preview the dataset and click **Save As**.

![](https://user-images.githubusercontent.com/12414122/51684180-8575a000-1fb9-11e9-8130-30f28232c825.png)

3\. Provide the **Table Title** and **Table ID** for the dataset and
click **Save.**

![](https://user-images.githubusercontent.com/12414122/51684181-8575a000-1fb9-11e9-8e1c-2f772c294493.png)

4\. Click **Done**.

![](https://user-images.githubusercontent.com/12414122/51684188-860e3680-1fb9-11e9-9e4d-6888181ad3c6.png)

Use these values to create the five needed datasets based on the
instructions above:

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Backup Job Events</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:eventfeed") | where eventType="Backup" | eval _time = strptime(time, "%a %b %d %H:%M:%S %Z %Y") | dedup id</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_backup_job_events</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
eventStatus<br>
locationName<br>
message<br>
objectId<br>
objectName<br>
objectType</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Runway Remaining</td>
</tr>
<tr class="even">
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:runwayremaining")</td>
</tr>
<tr class="odd">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_runway_remaining</td>
</tr>
<tr class="even">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
daysRemaining
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Security Audit Events</td>
</tr>
<tr class="even">
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:eventfeed") | where eventType="Audit" | eval _time = strptime(time, "%a %b %d %H:%M:%S %Z %Y")</td>
</tr>
<tr class="odd">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_security_audit_events</td>
</tr>
<tr class="even">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
eventStatus<br>
eventType<br>
message<br>
username<br>
hostname</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Storage Summary</td>
</tr>
<tr class="even">
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:storagesummary")</td>
</tr>
<tr class="odd">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_storage_summary</td>
</tr>
<tr class="even">
<td><strong>Fields</strong></td>
<td>_time<br>
available<br>
clusterName<br>
liveMount<br>
miscellaneous<br>
snapshot<br>
total<br>
used</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Cluster IO Stats</td>
</tr>
<tr class="even">
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:clusteriostats") | eval _time = strptime(time, "%Y-%m-%dT%H:%M:%S.%f%Z")</td>
</tr>
<tr class="odd">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_cluster_io_stats</td>
</tr>
<tr class="even">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
readBytePerSecond<br>
readsPerSecond<br>
writeBytePerSecond<br>
writesPerSecond</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Replication Events</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:eventfeed") | where eventType="Replication" | eval _time = strptime(time, "%a %b %d %H:%M:%S %Z %Y") | dedup id</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_replication_events</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
eventStatus<br>
message<br>
objectId<br>
objectName<br>
objectType</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Archive Events</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:eventfeed") | where eventType="Archive" | eval _time = strptime(time, "%a %b %d %H:%M:%S %Z %Y") | dedup id</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_archive_events</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
eventStatus<br>
message<br>
objectId<br>
objectName<br>
objectType</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Recovery Events</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:eventfeed") | where eventType="Recovery" | eval _time = strptime(time, "%a %b %d %H:%M:%S %Z %Y") | dedup id</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_recovery_events</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
eventStatus<br>
message<br>
objectId<br>
objectName<br>
objectType<br>
username</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Archive Bandwidth Usage</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:archivebandwidth") | eval _time = strptime(time, "%Y-%m-%dT%H:%M:%S.%f%Z")</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_archive_bandwidth_usage</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
locationName<br>
type<br>
value</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Archive Location Usage</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:archiveusage")</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_archive_usage</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
dataArchived<br>
dataDownloaded<br>
locationName<br>
numFilesetsArchived<br>
numHypervVmsArchived<br>
numLinuxFilesetsArchived<br>
numManagedVolumesArchived<br>
numMssqlDbsArchived<br>
numNutanixVmsArchived<br>
numShareFilesetsArchived<br>
numStorageArrayVolumeGroupsArchived<br>
numVMsArchived<br>
numWindowsVolumeGroupsArchived</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Node IO Stats</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:nodeiostats") | fields "_time", "clusterName", "nodeId", "readBytePerSecond", "readsPerSecond", "time", "writeBytePerSecond", "writesPerSecond" | eval _time = strptime(time, "%Y-%m-%dT%H:%M:%S.%f%Z") | fields "_time", "clusterName", "nodeId", "readBytePerSecond", "readsPerSecond", "writeBytePerSecond", "writesPerSecond"</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_node_io_stats</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
nodeId<br>
readBytePerSecond<br>
readsPerSecond<br>
writeBytePerSecond<br>
writesPerSecond</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Organisation Capacity Report</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:orgcapacityreport")</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_node_io_stats</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
ArchiveStorage<br>
ArchiveStorageGrowth<br>
cluster_name<br>
DataReductionPercent<br>
LocalDataReductionPercent<br>
LocalLogicalBytes<br>
LocalLogicalDataReductionPercent<br>
LocalStorage<br>
LocalStorageGrowth<br>
LogicalDataProtected<br>
LogicalDataReductionPercent<br>
Month<br>
Organization<br>
OrganizationId<br>
OrganizationState<br>
ReplicaStorage<br>
ReplicaStorageGrowth<br>
TotalArchiveStorage<br>
TotalLocalStorage<br>
TotalReplicaStorage</td>
</tr>
</tbody>
</table>


<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Managed Volume Summary</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:mvsummary")</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_managed_volume_summary</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterName<br>
count<br>
exported<br>
nfs<br>
smb<br>
writable</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Rubrik - Node Stats</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>(index="main") (sourcetype="rubrik:nodestats") | eval _time = strptime(time, "%Y-%m-%dT%H:%M:%S.%f%Z")</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>rubrik_dataset_node_io_stats</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
bytesReceived<br>
bytesTransmitted<br>
clusterName<br>
cpuCores<br>
cpuStat<br>
hdd_active_count<br>
hdd_inactive_count<br>
ipAddress<br>
isTunnelEnabled<br>
nodeId<br>
ram<br>
readBytePerSecond<br>
readsPerSecond<br>
sda_capacityBytes<br>
sda_diskType<br>
sda_isDegraded<br>
sda_path<br>
sda_status<br>
sda_unallocatedBytes<br>
sda_usableBytes<br>
sdb_capacityBytes<br>
sdb_diskType<br>
sdb_isDegraded<br>
sdb_path<br>
sdb_status<br>
sdb_unallocatedBytes<br>
sdb_usableBytes<br>
sdc_capacityBytes<br>
sdc_diskType<br>
sdc_isDegraded<br>
sdc_path<br>
sdc_status<br>
sdc_unallocatedBytes<br>
sdc_usableBytes<br>
sdd_capacityBytes<br>
sdd_diskType<br>
sdd_isDegraded<br>
sdd_path<br>
sdd_status<br>
sdd_unallocatedBytes<br>
sdd_usableBytes<br>
ssd_active_count<br>
ssd_inactive_count<br>
status<br>
time<br>
writeBytePerSecond<br>
writesPerSecond</td>
</tr>
</tbody>
</table>

### Configure Monitoring for Polaris

Configuring monitoring for Polaris is very similar to CDM. If you
skipped the CDM section, take a moment to review it so you are familiar
with the concepts. To configure credentials for Polaris, click on
**Apps** → **Rubrik Splunk Add-On**. Click on **Configuration**, then
click **Add**. Supply credentials with permissions to access Polaris.

#### Creating Input for Polaris

You will create one new input for Polaris. From within the Rubrik Splunk
Add-On, click **Inputs**, then **Create New Input**, then **Polaris -
Radar Anomales**.

![](https://user-images.githubusercontent.com/12414122/51684182-8575a000-1fb9-11e9-8b0d-08f6f9bf3f11.png)

Use these values to configure the input:

| **Input Type**     | Polaris - Radar Anomalies                    |
| ------------------ | -------------------------------------------- |
| **Name**           | polaris\_radar\_anomalies                    |
| **Interval**       | 900                                          |
| **Index**          | main                                         |
| **Global Account** | \<Polaris Account Name\>                     |
| **Polaris URL**    | \<your\_polaris\_url\>.my.rubrik.com         |

## Upgrading the Rubrik Add-On for Splunk

Upgrades for the Rubrik Splunk Add-On can be performed directly through
the Splunk GUI by clicking on **Apps** → **Manage Apps**, and clicking
**Update** for the Rubrik Splunk Add-On. Upgrades for the Rubrik Splunk
Application will be available via the same method. Review the inputs
and datasets section above, comparing them against the current configuration
to identify changes in fields, queries, and new datasets and inputs.

## Usage

There are five dashboards included with the Rubrik Add-On for Splunk:

* **Rubrik - Capacity Dashboard**: Displays capacity and throughput statistics for the cluster

* **Rubrik - Job History Dashboard**: Displays 24 hours of backup history, including successful and failed job statistics, object type breakdown, and failure logs

* **Rubrik - Security Dashboard**: Displays 24 hours of login information, top 10 logins my username and login count, and top 10 failed logins

* **Rubrik - Recovery History Dashboard**: Displays 24 hours of recovery information for the selected cluster

* **Rubrik - Archive and Replication Dashboard**: Displays 24 hours of information around arcival and replication from the selected cluster

To view dashboards, browse to **Apps** → **Rubrik**, and click on
**Dashboards**. Click on the desired dashboard, and select a cluster
from the **Cluster Name** dashboard. The dashboard will populate with
data that Splunk has gathered. Below is an example of the **Rubrik -
Capacity Dashboard**.

![](https://user-images.githubusercontent.com/12414122/51684187-8575a000-1fb9-11e9-8bfd-5c4c84ce924c.png)

The table below show the default timeframe for the **Rubrik - Capacity
Dashboard** components. All other dashboards components show 24 hours of
data.

| Widget                                | Timeframe     |
| ------------------------------------- | ------------- |
| Capacity Available                    | Latest data   |
| Capacity Available - Percentage       | Last 24 hours |
| Runway Remaining - Days               | Last 24 hours |
| Runway Remaining                      | Last 90 days  |
| Cluster Throughput - IOPS             | Last 24 hours |
| Cluster Throughput - Bytes per Second | Last 24 hours |

Further custom dashboards can be created as required, using standard Splunk queries. Some examples and suggestions are documented in the included [Custom Dashboard Guide](./custom-dashboard-guide.md).

### Setting a Default Rubrik Cluster for Dashboards

The bundled dashboards include a dropdown to select the cluster for
which to display gathered metrics. By default, this dropdown will be
blank. To set a default value, follow these steps:

Browse to the dashboard you wish to configure, and click the **Edit**
button.

![](https://user-images.githubusercontent.com/12414122/51684183-8575a000-1fb9-11e9-9c70-3ecfa11877fe.png)

This will open the **Edit Dashboard** page. Click the pencil icon above
the **Cluster Name** dropdown. In the displayed dialog, choose the
desired default cluster from the **Default** dropdown in the **Token
Options** section, then click **Apply**. Click **Save** on the **Edit
Dashboard** page to save your
changes.

![](https://user-images.githubusercontent.com/12414122/51684184-8575a000-1fb9-11e9-8eec-caba4b54ea79.png)

| Note: The list populating the `Default` dropdown may take some time to populate after new inputs are added to Splunk. |
| --- |

Check the [GitHub repo](https://github.com/rubrikinc/rubrik-splunk-addon) for more information on the Rubrik Add-On for Splunk.

## Additional Reading

* [\[BLOG\] API First: Introducing the New Splunk Add-on for Rubrik](https://www.rubrik.com/blog/api-splunk-add-on-rubrik/)
* [Splunkbase: Rubrik Splunk Add-On](https://splunkbase.splunk.com/app/4119/#/details)
* [GitHub repo: Rubrik AddOn for Splunk](https://github.com/rubrikinc/rubrik-splunk-addon)
* [\[BLOG\] What are Splunk Apps and Add-Ons?](https://www.splunk.com/blog/2014/07/22/what-are-splunk-apps-and-add-ons.html)
