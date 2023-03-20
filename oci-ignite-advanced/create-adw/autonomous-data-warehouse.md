# Deploying and configuring Autonomous Data Warehouse

## Introduction

Oracle Autonomous Data Warehouse Cloud provides an easy-to-use, fully autonomous database that scales elastically, delivers fast query performance and requires no database administration. In this hands on lab, we will walk through deploying an Autonomous Data Warehouse database and loading a table using a text file that is stored in object storage. 

The purpose of this lab is to get familiar with Oracle Autonomous Data Warehouse primitives. At the end of this lab, you will be familiar with launching an Autonomous Data Warehouse database, using secure network access control list and connect to the instance via SQL Developer tool.

Estimated Time: 5 minutes

### Objectives

In this lab, you will:

-   Provision a new Autonomous Database
-   Connect to it via SQL Developer
### Prerequisites

* This lab requires completion of the **Get Started** section in the Contents menu on the left.
* [Familiarity with OCI console](https://docs.oracle.com/en-us/iaas/Content/GSG/Concepts/console.htm)
* [Overview of Networking](https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/overview.htm)
* [Oracle Cloud basics](https://docs.oracle.com/en-us/iaas/Content/GSG/Concepts/concepts.htm)
* [Connecting to a compute instance](https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/accessinginstance.htm)
* Your **<font color="red">Oracle Cloud Account</font>** - During this workshop we will create a Level 100 environment for you to use on your tenancy.

## Task 1: Choose Autonomous Database from the Services Menu

1. Log in to the Oracle Cloud Interface.
2. Once you log in, you arrive at the cloud services dashboard where you can see all the services available to you. Click the navigation menu in the upper left to show top level navigation choices and then click **Autonomous Database**.

    ![Oracle home page.](images/navigate-adb.png " ")

## Task 2: Create the Autonomous Database instance

1. Click **Create Autonomous Database** to start the instance creation process.

    ![Click Create Autonomous Database.](images/click-create-adb.png " ")

2.  This brings up the __Create Autonomous Database__ screen where you will specify the configuration of the instance.

 
3. Give basic information for the Autonomous Database:

    - __Choose a compartment__ - Choose the compartment **Workshop** created in __Ignite Basics__. See [the documentation](https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managingcompartments.htm) if you want to learn more about compartments.
    - __Display Name__ - Enter a memorable name for the database for display purposes. For this lab, use __Ignite ADW__.
    - __Database Name__ - Use letters and numbers only, starting with a letter. Maximum length is 14 characters. (Underscores not initially supported.) For this lab, use __IGNITEADW__.

    ![Enter the required details.](images/create-adb-configure-details.png" ")


4. Choose __Data Warehouse__ workload type:

    ![Choose a workload type.](images/choose-adb-workload-type.png " ")

5. Choose __Shared Infrastructure__ deployment type:

    ![Choose a deployment type.](images/choose-adb-deployment-type.png " ")

6. Configure the database:

    - __Always Free__ - If your Cloud Account is an Always Free account, you can select this option to create an always free autonomous database. An always free database comes with 1 CPU and 20 GB of storage. For this lab, we recommend you leave Always Free unchecked.
    - __Choose database version__ - Select 19c as the database version.
    - __OCPU count__ - Number of CPUs for your service. For this lab, specify __1 CPUs__. If you choose an Always Free database, it comes with 1 CPU.
    - __Storage (TB)__ - Select your storage capacity in terabytes. For this lab, specify __1 TB__ of storage. Or, if you choose an Always Free database, it comes with 20 GB of storage.
    - __Auto Scaling__ - For this lab, keep auto scaling enabled, to enable the system to automatically use up to three times more CPU and IO resources to meet workload demand.
    
    > **Note:** You cannot scale up/down an Always Free autonomous database.

    ![Choose the remaining parameters.](images/configure-adb.png " ")

7. Create administrator credentials:

    - __Password and Confirm Password__ - Specify the password for ADMIN user of the service instance. The password must meet the following requirements:
    - The password must be between 12 and 30 characters long and must include at least one uppercase letter, one lowercase letter, and one numeric character.
    - The password cannot contain the username.
    - The password cannot contain the double quote (") character.
    - The password must be different from the last 4 passwords used.
    - The password must not be the same password that you set less than 24 hours ago.
    - Re-enter the password to confirm it. Make a note of this password.

    ![Enter password and confirm password.](images/adb-admin-credential.png " ")

8. Choose network access:
    
    - If you want to allow traffic from everywhere, select **Secure access from everywhere**.
    - If you want to allow traffic only from the IP addresses and VCNs you specify - where access to the database from all public IPs or VCNs is blocked, select **Secure access from allowed IPs and VCNs only** in the Choose network access area.
    - If you want to restrict access to a private endpoint within an OCI VCN, select **Private endpoint access only** in the Choose network access area.
    - If the **Require mutual TLS (mTLS) authentication** option is selected, mTLS will be required to authenticate connections to your Autonomous Database. TLS connections allow you to connect to your Autonomous Database without a wallet, if you use a JDBC thin driver with JDK8 or above. See the [documentation for network options](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/support-tls-mtls-authentication.html#GUID-3F3F1FA4-DD7D-4211-A1D3-A74ED35C0AF5) for options to allow TLS, or to require only mutual TLS (mTLS) authentication.

    - For this lab, select, **Secure access from allowed IPs and VCNs only**. Then under __IP notation type__ choose **Virtual Cloud Network** and select the **VCN-Workshop** created in *Lab 1: Create a Virtual Cloud Network*. 

    ![Choose the network access type.](images/adb-network-access.png " ")


9. Choose a license type. For this lab, choose __License Included__. The two license types are:
    - __Bring Your Own License (BYOL)__ - Select this type when your organization has existing database licenses.
    - __License Included__ - Select this type when you want to subscribe to new database software licenses and the database cloud service.

    ![Choose license type](images/adb-license-type.png" ")

10. For this lab, do not provide a contact email address. The "Contact Email" field allows you to list contacts to receive operational notices and announcements as well as unplanned maintenance notifications.

11. Click __Create Autonomous Database__.

 ![Click on Create Autonomous Database](images/click-create-adb2.png " ")

12.  Your instance will begin provisioning. In a few minutes, the state will turn from Provisioning to Available. At this point, your Autonomous Database is ready to use! Have a look at your instance's details here including its name, database version, OCPU count, and storage size.

    ![Database instance homepage.](images/adb-state.png " ")
    Provisioning an Autonomous Database instance.
## Task 3: Connect to the Autonomous Database instance via SQL Developer

1. Click on **Edit** in the **Network** category, **Access control list** section:

    ![Edit Access control list](images/modify-access-control-list-adb.png " ")

2. Click on **Add access control rule**:

    ![Add access control rule](images/adb-add-access-control-rule.png " ")

3. Click on **Add my IP address** and click **Save** on the bottom of the window:

    ![Add your IP address](images/adb-access-control-list-add-myip.png " ")

4. Click on **Database connection**:

    ![Access Database connection options](images/adb-database-connection.png " ")

4. Under **Wallet type**, choose **Instance Wallet** and  click on **Download wallet**. 

    ![Download client credentials (Wallet)](images/adb-database-connection-download-wallet.png " ")

5. Please create a **password** for this wallet. Some database clients will require that you provide both the wallet and password to connect to your database (other clients will auto-login using the wallet without a password).

    ![Choose a password for Wallet](images/adb-download-wallet-password.png " ")

    Choose a location to save the *wallet.zip* file.

    Click the **Close** button at the bottom of the screen.

6. Open SQL Developer tool, and click on the **green cross** to add a new/select database connection:

    ![Add a new connection in SQL Developer](images/new-connection-sqldeveloper.png " ")

7. Configure the database connection:

    - __Name__ - Enter the name *IGNITEADW* of the created database.
    - __Add the credentials__ - Fill in the username *ADMIN* and password *Oracle123!!!* chosen when we provisioned the database (__*Task 2, Step 7*__)
    - __Connection Type__ - For this lab, specify __Cloud Wallet__. Browse the location where you saved the *wallet.zip* in __*Step 5*__ and **select the .zip file**.
    - __Storage (TB)__ - Select your storage capacity in terabytes. For this lab, specify __1 TB__ of storage. Or, if you choose an Always Free database, it comes with 20 GB of storage.
    - __Auto Scaling__ - For this lab, keep auto scaling enabled, to enable the system to automatically use up to three times more CPU and IO resources to meet workload demand.
    
    > **Note:** You cannot scale up/down an Always Free autonomous database.

    ![Configure the database connection parameters.](images/configure-connection-sqldeveloper.png " ")




    




Please *proceed to the next lab*.

## Learn more

See the [documentation](https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/user/autonomous-workflow.html#GUID-5780368D-6D40-475C-8DEB-DBA14BA675C3) on the typical workflow for using Autonomous Data Warehouse.


## Acknowledgements
*Congratulations! You have successfully completed the lab.*

- **Author** - Cristian Manea
- **Last Updated By/Date** - Cristian Manea, February 2022

