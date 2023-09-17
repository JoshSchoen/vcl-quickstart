# SQL Tutorial: Creating Compute Groups and Profiles

In this tutorial, you will learn how to create Compute Groups and Profiles in a VantageCold Lake database. Compute Groups & Profile is crucial for controlling workloads for departments applications, or projects, and then segregate processing, monitoring, reporting, and billing by department, application, or project VantageCold Lake environment. [Learn more about Compute groups and profiles](https://docs.teradata.com/r/Teradata-VantageCloud-Lake/Using-VantageCloud-Lake-Console-to-Manage-VantageCloud-Lake/Managing-Environments/Compute-Groups/Creating-a-Compute-Group).

## Prerequisites

Before you begin, make sure you have the following:

- Access to a VantageCould Lake Console. If you need access contact your administrator or [start a trial]().
- Access to a VantageCold Lake Environment. Creating an environment is easy, here's a [tutorial]() to get you started.
- Appropriate privileges or access to an account with sufficient permissions to create compute groups and profiles.

## Step 1: Connect to the Environment

Use VantageCould Lake Console Editor, a Teradata SQL client, command-line tool or to connect to the Environments database where you want to create the user. Provide the necessary connection details,
`Server name`, `username`, `password`.

## Step 2: Create a Compute group

```
/***********************************
2. Create Clusters:
 - CG_Standard - XS profile
 - CG_Analytic - XS profile
************************************/
​
CREATE COMPUTE GROUP "CG_Standard" USING QUERY_STRATEGY('STANDARD');
```

## Step 3: Create a Compute profile

Use the `CG_Standard` compute group create a compute group profile.

```
CREATE COMPUTE PROFILE "XS_Profile" IN COMPUTE GROUP "CG_Standard"
,INSTANCE = TD_COMPUTE_XSMALL
,INSTANCE TYPE = STANDARD
 USING
     MIN_COMPUTE_COUNT (1)
     MAX_COMPUTE_COUNT (1)
     SCALING_POLICY ('STANDARD')
     START_TIME ('0 13 * * MON-FRI')
     END_TIME ('0 01 * * MON-SAT')
     COOLDOWN_PERIOD (30);
```

The SQL script above creates a compute group profile. The compute profile defines the number of clusters, the degree of autoscale, and the schedule window for the compute group when the clusters run. A compute group can have multiple compute profiles for different times of day or different workloads.

## Step 4: Create another Compute group and profile

This step is optional but required to complete the [The Kevin flow 2.0]() tutorial.

Repeat steps 2 & 3 and create a second compute group and profile using compute group name `CG_Analytic` and profile name `XS_Profile` for the new `CG_Analytic` compute group.

Congratulations! You've learned how to create a a compute group in a VantageCloud Lake environment, and assign a compute group profile. Compute groups are optional but recommended to improve separation of workloads and monitoring, reporting, and billing.

[Previous: Creating an Environment]() | [Next: Creating users](https://github.com/JoshSchoen/vcl-quickstart/blob/main/create-users)
