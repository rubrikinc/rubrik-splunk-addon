# Rubrik Splunk Add-On: Custom Dashboard Guide

## Overview

This document is intended to provide example searches and ideas for customers wishing to customise dashboards in Splunk using the Rubrik Splunk application.

The application provides some example dashboards out of the box, but many customers wish to have more specific data presented in a visual format. Here we discuss some ideas for such customisations, as well as provide sample searches for these ideas.

## Per-Object Type Reporting Using Dashboard Inputs

This describes adding a dropdown to the dashboard to filter the resultant panels based on object type.

### Adding a dropdown

The following dropdown can be added to the 'Job History Dashboard' dashboard:

![Object Type Dropdown](./images/objecttype_dropdown.png)

The following search string can be used to generate this list:

```none
| from datamodel:"rubrik_dataset_backup_job_events" | where clusterName=="$clusterName$" |  table objectType | dedup objectType
```

This input needs to be given a token name, for the remainder of this example we will use the value `objectType` (this is set up during creation of the dashboard input).

### Using the input to feed searches

Now the input is set up, it can be used to feed searches in the other panels in the dashboard. The pre-created searches can be modified by adding a `where` clause to the query. An example of this is below for the 'Last 24 Hours Success Percentage' panel:

```none
| from datamodel:"rubrik_dataset_backup_job_events" | where clusterName=="$clusterName$" | where objectType=="$objectType$" | stats count(eval(eventStatus="Success")) as Success count(eval(eventStatus=="Failure")) as Failure | eval percent_difference=((Success/(Success+Failure))*100) | fields percent_difference
```

From this, we can see the clause for using the input is:

```none
where objectType=="$objectType$"
```

This can be added in-line to the existing query.