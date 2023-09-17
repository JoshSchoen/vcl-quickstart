# SQL Tutorial: Creating Users

In this tutorial, you will learn how to create users in a VantageCloud Lake database. User management is crucial for controlling access and security within your VantageCold Lake environment.

## Prerequisites
Before you begin, make sure you have the following:
- Access to a VantageCloud Lake Console. If you need access contact your administrator or [start a trial]().
- Access to a VantageCloud Lake Environment. Creating an environment is easy, here's a [tutorial]() to get you started.
- Appropriate privileges or access to an account with sufficient permissions to create users.

## Step 1: Access the VantageCloud Lake Environments Database
Use VantageCloud Lake Console Editor a Teradata SQL client, command-line tool or  to connect to the Environments database where you want to create the user. Provide the necessary connection details,
`Server name`, `username`, `password`

---

*If you don't have access to a SQL client or a command-line, the **VantageCloud Lake Console Editor** is the easiest way to get started. Contact your administrator or [start a trial]().*

---

## Step 2: Create a New User from the DBC User
To create a new user in VantageCloud Lake, you will use the `CREATE USER` statement. Replace and `password` with the desired `username` and `password` for your user. We recommend using `trials_user`, but you can specific any username right after the `CREATE USER` statement.

---
#### What is the DBC user?
You can think of the DBC user are the root users and is required to create an environment, The DBC users is not able to login in to the VantageCloud Lake Console so we must create a database user to work with the environments database.
[Learn more about what a DBC users]() and how to create it during environment provisioning.

---
### Create User from DBC
```
/***********************************
1. Create User from DBC
trials_user/password
************************************/
​
CREATE USER trials_user FROM DBC
  AS PERM  = 1E11          /* 100GB */
    ,SPOOL = 8E11          /* 800GB */
    ,PASSWORD = "password"
    ,ACCOUNT = ('$M')
    ,TIME ZONE = '00:00'   /* UTC */
    ,DEFAULT ROLE = ALL;
```

---

### Optional Setup 

Step 3 & 4: Define user permissions and grant administrative Permissions to `trial_user`

---

## Step 3: Define User Permissions


**Optional:** Now that you have created a role and granted it privileges, you can assign the role to the user you created:

```
/***********************************
Grant All privileges on trials_user to itself.
************************************/

GRANT all on "trials_user" to "trials_user" WITH GRANT OPTION;

​/***********************************
 --Grant system-level privileges to SYSDBA.
 ************************************/

GRANT MONRESOURCE, MONSESSION, ABORTSESSION, SETSESSRATE, SETRESRATE, REPLCONTROL,
	CREATE PROFILE, CREATE ROLE, DROP PROFILE, DROP ROLE 
TO "trials_user" WITH GRANT OPTION;
​
GRANT EXECUTE, SELECT, STATISTICS, SHOW on DBC to "trials_user" WITH GRANT OPTION;
GRANT UDTTYPE, UDTMETHOD on SYSUDTLIB to "trials_user" WITH GRANT OPTION;
GRANT SELECT, INSERT, UPDATE, DELETE on "Sys_Calendar" to "trials_user" WITH GRANT OPTION;
GRANT SELECT, EXECUTE FUNCTION, EXECUTE PROCEDURE on TD_SYSFNLIB to "trials_user" WITH GRANT OPTION;
GRANT SELECT, EXECUTE FUNCTION, EXECUTE PROCEDURE on TD_MLDB to "trials_user" WITH GRANT OPTION;
GRANT SELECT, EXECUTE FUNCTION, EXECUTE PROCEDURE on SYSLIB to "trials_user" WITH GRANT OPTION;
GRANT EXECUTE FUNCTION on TD_SYSFNLIB.READ_NOS to "trials_user" WITH GRANT OPTION;
GRANT CREATE DATASET SCHEMA on SYSUDTLIB to "trials_user" WITH GRANT OPTION;
GRANT SELECT on TD_METRIC_SVC to "trials_user" WITH GRANT OPTION;
GRANT SELECT, EXECUTE FUNCTION, EXECUTE PROCEDURE on SYSSPATIAL to "trials_user" WITH GRANT OPTION;
```
## Step 4: Grant trail_user the privileges to grant others to System Roles.
**Optional:** Now that you have created a role and granted it privileges, you can grant others to System Roles.

```
​/***********************************
Grant trials_user the privileges to grant others to System Roles.
************************************/

GRANT  TD_USER_ADMIN
      ,TD_DATA_SCIENTIST
      ,TD_FLOW_COMPOSER
      ,TD_CURRENT_QUERIES_ADMIN
      ,TD_CURRENT_QUERIES_VIEW
      ,TD_QUERY_HISTORY_VIEW
      ,TD_COMPUTE_CLUSTER_ADMIN
TO "trials_user" WITH ADMIN OPTION;
​
--Compute group-specific permissions
GRANT DROP COMPUTE PROFILE to "trials_user" WITH GRANT OPTION;
GRANT CREATE COMPUTE PROFILE to "trials_user" WITH GRANT OPTION;
GRANT CREATE COMPUTE GROUP to "trials_user" WITH GRANT OPTION;
GRANT DROP COMPUTE GROUP to "trials_user" WITH GRANT OPTION;
​
GRANT LOGON ON ALL TO "trials_user" WITH NULL PASSWORD;
```
## Step 5: Verify User Creation
You can verify that the user has been created successfully by querying the Teradata system tables. For example:

```
SELECT * FROM DBC.UsersV WHERE UserName = 'trial_user';
```
This query will return user information for the newly created user.


## Step 6: Test the User

***TODO: show how to in editor and other SQL clients***

Finally, test the new user by connecting to the database using their credentials:


```
bteq -u username -p
```

Congratulations! You've learned how to create a user in a VantageCloud Lake environment, assign roles and permissions, and verify the user's existence. User management is crucial for maintaining security and access control in your VantageCloud Lake environment.
