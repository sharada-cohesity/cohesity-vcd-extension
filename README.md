# Cohesity Extension for VMware vCloud Director

Cohesity Extension for VMware vCloud Director enables cloud providers to offer data protection as a service in multi-tenant environments. This extension integrates natively with vCloud Director HTML UI and makes self-service data protection  available to tenants. It delivers self-service in a secure manner through role-based access control. Having integrated data protection services not only streamlines operations but also provides rich tenant experience, allowing cloud providers to monetize services.

## Prerequisites

Software Required:

* vCloud Director: 9.5 or later
* Cohesity Data Platform: 6.2 or later
* Web browsers: Google Chrome, Firefox, Safari
* Yarn: 1.1 or later (To install Plugin Lifecycle Management)

## Download
Please [click here](https://github.com/cohesity/cohesity-vcd-extension/releases/latest) to download Cohesity Extension for VMware vCloud Director.

## Installation Steps

This section describes the detailed steps to install and configure the Cohesity Extension for VMware vCloud Director.

### 1. Enable CORS on the Cohesity Cluster
vCloud Director extension makes cross-origin API requests to the Cohesity Cluster.
To enable support for CORS on the Cohesity Cluster, please follow the steps below.

1) Open the Cohesity Cluster UI and click on the "Download CLI" link in the footer. Download the `iris_cli` binary and make sure it has executable permissions.
2) Connect to the Cohesity Cluster using `iris_cli`
```
iris_cli -server {cohesity_cluster} -username {username} -password {password}
```
3) Once you are connected and in the shell, replace `vcd_hostname` with the hostname of your vCloud Director server in the command below and run it in the shell to enable CORS
```
cluster update-gflag gflag-name="iris_cors_origins" gflag-value="https://{vcd_hostname}" service-name=iris reason="Enabling_CORS"
```
4) Restart `iris` service by running the command below in the shell
```
cluster restart service-names=iris
```
5) Exit the shell
```
exit
```

### 2. Enable organizations on the Cohesity Cluster
To enable organizations on the Cohesity Cluster please follow the steps below:

1) Login to the Cohesity Cluster UI as a user with Admin role
2) Click on "Admin->Cluster Settings"
3) Click on the toggle buton to "Enable Organizations"
4) Create organizations in Cohesity corresponding to the tenants in vCloud Director by navigating to "Admin->Organizations" and clicking on "Add Organization"

### 3. Install VMware's Plugin-Lifecycle Management extension

Plugin-Lifecycle management is a tool provided in vCD Developer SDK that enables managing the plugin/extension lifecycle i.e. install, enable/disable, remove, list all extensions in vCD.

#### Steps (Install Plugin Lifecycle Management):

1) Clone vCD git repository. 

    `git clone https://github.com/vmware/vcd-ext-sdk.git`

2) Navigate to local cloned repository and change directory to `vcd-ext-sdk/ui/plugin-lifecycle`

    `cd vcd-ext-sdk/ui/plugin-lifecycle`

3) Rename `ui_ext_api.ini.template` to `ui_ext_api.ini` and configure below details:

    ```
    [DEFAULT]
    * vcduri=<vCD IP>
    * username=<admin username>
    * organization=System
    * password=<password>
    ```

4) Run the following commands from `plugin-lifecycle` directory.
```
yarn
yarn build
yarn deploy
```

5) Now "Plugin Lifecycle Management" extension will be visible in provider scope of vCD.


### 4. Install Cohesity Extension:

1) Navigate to vCD and login as a provider.
	
    ![alt-text](/documentation/images/image5.png)

2) Click on hamburger icon and select "Plugin Lifecycle Management"

    ![alt-text](/documentation/images/image6.png)

3) Click on upload.

    ![alt-text](/documentation/images/image7.png)

4) Click on “Select zip To Upload” and Navigate to the `cohesity_vcd_extension_{version}.zip` file for cohesity extension.

    ![alt-text](/documentation/images/image8.png)

5) Follow the instructions on screen to upload the plugin.

    ![alt-text](/documentation/images/image9.png)

6) On success, the uploaded extension will be visible under "Data Protection" when you click on the Hamburger Icon.

7) Service Provider must first add Cohesity Cluster configuration from the provider view by clicking on "Data Protection" and also specify the mapping of vCloud tenants to Cohesity organizations. Please note that prior to this "Enable Organizations" setting must be enabled on the Cohesity Cluster and organizations must be present on the Cluster for the mapping to succeed.

8) Once this is done, tenants can start using the extension by clicking on "Data Protection" in the tenant view.


## Features

1) Overview
    * Provides summary of the vCD resources and the associated cohesity protection status. 

2) Protection for vApps/VMs
    * Protect and unprotect VMs and vApps
    * On-demand backups for VMs and vApps.

3) Restore
    * Recover files, folders or VM/vApp from backed up objects

4) Monitor
    * Monitoring backup and restore tasks executed from vCD extension.


## Feedback
We love to hear from you. Please send your feedback and suggestions to cohesity-api-sdks@cohesity.com
