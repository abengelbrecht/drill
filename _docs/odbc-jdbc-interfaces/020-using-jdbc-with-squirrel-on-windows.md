---
title: "Using JDBC with SQuirreL on Windows"
parent: "ODBC/JDBC Interfaces"
---
To use the JDBC Driver to access Drill through SQuirreL, ensure that you meet the prerequisites and follow the steps in this section.
### Prerequisites

  * SQuirreL requires JRE 7
  * Drill installed in distributed mode on one or multiple nodes in a cluster. Refer to the [Install Drill]({{ site.baseurl }}/docs/install-drill/) documentation for more information.
  * The client must be able to resolve the actual hostname of the Drill node(s) with the IP(s). Verify that a DNS entry was created on the client machine for the Drill node(s).
     
If a DNS entry does not exist, create the entry for the Drill node(s).

    * For Windows, create the entry in the %WINDIR%\system32\drivers\etc\hosts file.
    * For Linux and Mac, create the entry in /etc/hosts.  
<drill-machine-IP> <drill-machine-hostname>
    Example: `127.0.1.1 maprdemo`

----------

### Step 1: Getting the Drill JDBC Driver

The Drill JDBC Driver `JAR` file must exist in a directory on your Windows
machine in order to configure the driver in the SQuirreL client.

You can copy the Drill JDBC `JAR` file from the following Drill installation
directory on the node with Drill installed, to a directory on your Windows
machine:

    <drill_installation_directory>/jars/jdbc-driver/drill-jdbc-all-<version>.jar

Or, you can download the [apache-
drill-1.0.0.tar.gz](http://apache.osuosl.org/drill/drill-1.0.0/apache-drill-1.0.0.tar.gz) file to a location on your Windows machine, and
extract the contents of the file. You may need to use a decompression utility,
such as [7-zip](http://www.7-zip.org/) to extract the archive. Once extracted,
you can locate the driver in the following directory:

    <windows_directory>\apache-drill-<version>\jars\jdbc-driver\drill-jdbc-all-<version>.jar

----------

### Step 2: Installing and Starting SQuirreL

To install and start SQuirreL, complete the following steps:

  1. Download the SQuirreL JAR file for Windows from the following location:  
<http://www.squirrelsql.org/#installation>
  2. Double-click the SQuirreL `JAR` file. The SQuirreL installation wizard walks you through the installation process.
  3. When installation completes, navigate to the SQuirreL installation folder and then double-click `squirrel-sql.bat` to start SQuirreL.

----------

### Step 3: Adding the Drill JDBC Driver to SQuirreL

To add the Drill JDBC Driver to SQuirreL, define the driver and create a
database alias. The alias is a specific instance of the driver configuration.
SQuirreL uses the driver definition and alias to connect to Drill so you can
access data sources that you have registered with Drill.

#### A. Define the Driver

To define the Drill JDBC Driver, complete the following steps:

1. In the SQuirreL toolbar, select **Drivers > New Driver**. The Add Driver dialog box appears.
  
    ![drill query flow]({{ site.baseurl }}/docs/img/40.png)

2. Enter the following information:

    | Option           | Description                                                                                                                                                                                                          |
    |------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Name             | Name for the Drill JDBC Driver                                                                                                                                                                                       |
    | Example URL      | jdbc:drill:zk=<zookeeper_quorum>[;schema=<schema_to_use_as_default>]Example: jdbc:drill:zk=maprdemo:5181Note: The default ZooKeeper port is 2181. In a MapR cluster, the ZooKeeper port is 5181.                     |
    | Website URL      | jdbc:drill:zk=<zookeeper_quorum>[;schema=<schema_to_use_as_default>]Example: jdbc:drill:zk=maprdemo:5181Note: The default ZooKeeper port is 2181. In a MapR cluster, the ZooKeeper port is 5181.                     |
    | Extra Class Path | Click Add and navigate to the JDBC JAR file location in the Windows directory:<windows_directory>/jars/jdbc-driver/drill-jdbc-all-0.6.0-incubating.jar Select the JAR file, click Open, and then click List Drivers. |
    | Class Name       | Select org.apache.drill.jdbc.Driver from the drop-down menu.                                                                                                                                                         |
  
3. Click **OK**. The SQuirreL client displays a message stating that the driver registration is successful, and you can see the driver in the Drivers panel.  

   ![drill query flow]({{ site.baseurl }}/docs/img/52.png)

#### B. Create an Alias

To create an alias, complete the following steps:

1. Select the **Aliases** tab.
2. In the SQuirreL toolbar, select **Aliases >****New Alias**. The Add Alias dialog box appears.
    
    ![drill query flow]({{ site.baseurl }}/docs/img/19.png)
    
3. Enter the following information:
  
     <table style='table-layout:fixed;width:100%'><tbody><tr>
     <td valign="top" width="10%"><strong>Option</strong></td>
     <td valign="top" style='width: 500px;'><strong>Description</strong></td>
     </tr>
     <tr>
     <td valign="top">Alias Name</td>
     <td valign="top">A unique name for the Drill JDBC Driver alias.</td>
     </tr>
     <tr>
     <td valign="top">Driver</td>
     <td valign="top">Select the Drill JDBC Driver.</td>
     </tr>
     <tr>
     <td valign="top">URL</td>
     <td valign="top">Enter the connection URL with the name of the Drill directory stored in ZooKeeper and the cluster ID:
       <code>jdbc:drill:zk=&lt;<em>zookeeper_quorum</em>&gt;/&lt;drill_directory_in_zookeeper&gt;/&lt;cluster_ID&gt;;schema=&lt;<em>schema_to_use_as_default</em>&gt;</code>
       <em>The following examples show URLs for Drill installed on a single node:</em><br />
       <span style="font-family: monospace;font-size: 14.0px;line-height: 1.4285715;background-color: transparent;">jdbc:drill:zk=10.10.100.56:5181/drill/demo_mapr_com-drillbits;schema=hive<br /></span>
       <span style="font-family: monospace;font-size: 14.0px;line-height: 1.4285715;background-color: transparent;">jdbc:drill:zk=10.10.100.24:2181/drill/drillbits1;schema=hive<br /> </span>
       <em>The following example shows a URL for Drill installed in distributed mode with a connection to a ZooKeeper quorum:</em>
       <span style="font-family: monospace;font-size: 14.0px;line-height: 1.4285715;background-color: transparent;">jdbc:drill:zk=10.10.100.30:5181,10.10.100.31:5181,10.10.100.32:5181/drill/drillbits1;schema=hive</span>
          <ul>
          <li>Including a default schema is optional.</li>
          <li>The ZooKeeper port is 2181. In a MapR cluster, the ZooKeeper port is 5181.</li>
          <li>The Drill directory stored in ZooKeeper is <code>/drill</code>.</li>
          <li>The Drill default cluster ID is<code> drillbits1</code>.</li>
          </ul>
     </td></tr><tr>
     <td valign="top">User Name</td>
     <td valign="top">admin</td>
     </tr>
     <tr>
     <td valign="top">Password</td>
     <td valign="top">admin</td>
     </tr></tbody></table>
4. Click **Ok**. The Connect to: dialog box appears.  

    ![drill query flow]({{ site.baseurl }}/docs/img/30.png)
   
5. Click **Connect**. SQuirreL displays a message stating that the connection is successful.
  
    ![drill query flow]({{ site.baseurl }}/docs/img/53.png)
     
6. Click **OK**. SQuirreL displays a series of tabs.

----------

### Step 4: Running a Drill Query from SQuirreL

Once you have SQuirreL successfully connected to your cluster through the
Drill JDBC Driver, you can issue queries from the SQuirreL client. You can run
a test query on some sample data included in the Drill installation to try out
SQuirreL with Drill.

To query sample data with Squirrel, complete the following steps:

1. Click the SQL tab.
2. Enter the following query in the query box:   
   
        SELECT * FROM cp.`employee.json`;
          
     Example:  
     ![drill query flow]({{ site.baseurl }}/docs/img/11.png)

3. Press **Ctrl+Enter** to run the query. The following query results display: 
  
     ![drill query flow]({{ site.baseurl }}/docs/img/42.png) 

You have successfully run a Drill query from the SQuirreL client.



