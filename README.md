# 101 Hadoop and HDFS

Module 1, Big Data course (81932), University of Bologna.

## 101-1 Cluster setup

Goal: setup connections to the classroom's cluster via Putty and WinSCOP

- Create a connection to isi-vclust**N**.csr.unibo.it on Putty
  - **N** is the number of the node you have been assigned to
- Get your credentials from https://tinyurl.com/bigdata20users 
- Change your password!
```passwd <username>```
- Create a directory in your home called bigdata
```mkdir <foldername>```
- Connect to isi-vclustN.csr.unibo.it via WinSCP
- Check the existence of the directory

## 101-2 HDFS disk usage

Goal: understand basic HDFS commands that provide reports on the disk usage

```
hdfs dfs -df -h
hdfs dfs -du -h /
hdfs dfsadmin -report
```

## 101-3 HDFS storing files

Goal: create/remove files and directories; navigate directories; change the replication factor of files.

### From shell

```shell
# Explore HDFS directories with –ls
hdfs dfs -ls /
# Create a bigdata folder in your HDFS home
hdfs dfs -mkdir bigdata
# Create a dummy file in your folder in the local file system
echo 'This is a dummy file' > dummy.txt
# Put the dummy file to your bigdata folder in HDFS
hdfs dfs -put bigdata/dummy.txt
# Change the replication factor of the dummy file to 5
hdfs dfs -setrep -w 5 bigdata/dummy.txt
# Verify that the number of replicas has actually increased
# Delete the test folder and the dummy.txt file on HDFS
hdfs dfs –rm -skipTrash bigdata/dummy.txt
```

### From HDFS's web UI

HDFS provides a basic web interface with read permissions on the filesystem. 

Go to [Cloudera Manager](http://137.204.72.233:7180/cmf/home) (Username: student - Password: student) > HDFS service (left panel) > NameNome Web UI > Utilities > Browse the file system. Navigate to your folder and click on your file to check blocks' locations and download the file.

### From Apache Hue

Apache Hue offers a more complete navigation of the filesystem, with the possibility to create/move/rename/delete folders and files. You can change permissions, download files, and use the drag&drop feature to easily upload new files and folders.

Go to [Apache Hue](http://137.204.72.233:8888) and click on the three-lines menu (top-left) > Files.

## 101-4 Virtual machine setup

Quickstart for CDH 5.13 is the virtual machine from Cloudera that provides a safe environment for testing and self-learning. It is already available on lab computers, but you can also download it from [here](https://www.cloudera.com/downloads/quickstart_vms/5-13.html) and install it on your computers.

### Minimum requirements

Minimum requirements depend on the desired configuration:
- 4 GB of RAM for running CDH alone
- 8 GB of RAM for running Cloudera Express (i.e., with Cloudera Manager)
- 12 GB of RAM for running Cloudera Enterprise (i.e., the commercial version)

We will use 6 GB of RAM to run Cloudera Express with a reduced number of services.

### Running CDH and Cloudera Manager

- Open VMware and make sure that the settings of the virtual machine comply with the minimum requirements.
- Launch the virtual machine.
- Go to System > Preferences > Keyboard > Layouts to setup the Italian keyboard
- Open a new Terminal and launch Cloudera Manager with the command ```sudo /home/cloudera/cloudera-manager --force --express```
- Open the browser and select the Cloudera Manager bookmark
- Stop every unnecessary service (i.e., HBase, Impala, Key-Value Store, Oozie, Solr, Sqoop 1 Client, Sqoop 2). Delete them if you don't wont them starting up again if the cluster is restarted.
- Remember: if you close the VM by suspending, some services may go down when the VM is reopened and will need to be restarted; otherwise, if you clode the VM by shutting down, Cloudera Manager will need to be restarted when the VM is reopened.

### Loading datasets

Put the content of the dataset folder in the virtual machine either by setting Git in the virtual machine, or by copy/pasting the folder from your physical machine.

Then, create a "dataset" folder in the home folder on HDFS and put there the files. Either use the following commands from a Terminal windows or use the web UI of Hue.

```shell
hdfs dfs -mkdir dataset
hdfs dfs -put <localpath1> ... <localpathN> dataset
```

## 101-5 Differences between Cluster and Virtual machine

- Java: 1.8 cluster, 1.7 VM
- CDH: 5.13 both
- Hadoop: 2.6 both
- Spark: 1.6 both
- Spark2: 2.1 cluster only
