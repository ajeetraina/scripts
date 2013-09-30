1.Create a user called Hadoop as shown below:

$ adduser hadoop

$ passwd hadoop

For this series, we will keep hadoop/ hadoop as credential.

Once logged in as Hadoop user, one can verify the userid with the below command:

$id

It will show hadoop as uid and gid which means you are good to go.

We are going to use hadoop-1.0.3 version for initial setup.

Download http://archive.apache.org/dist/hadoop/core/hadoop-1.0.3/hadoop-1.0.3.tar.gz and place it under /usr/local/ directory.

Go to /usr/local/ directory.

$sudo tar xvzf hadoop-1.0.3.tar.gz

$sudo mv hadoop-1.0.3 hadoop

HADOOP CONFIGURATION FILES:

$cd /usr/local/hadoop

$cd conf

$vi core-site.xml

<property>

<name>hadoop.tmp.dir</name>

<value>/app/hadoop/tmp</value>

<description>A base for other temporary directories.</description>

</property>

<property>

<name>fs.default.name</name>

<value>hdfs://localhost:54310</value>

<description>The name of the default file system. A URI whose

scheme and authority determine the FileSystem implementation. The

uri's scheme determines the config property (fs.SCHEME.impl) naming

the FileSystem implementation class. The uri's authority is used to

determine the host, port, etc. for a filesystem.</description>

</property>

===================================================

#vi mapred-site.xml

<property>

<name>mapred.job.tracker</name>

<value>localhost:54311</value>

<description>The host and port that the MapReduce job tracker runs

at. If "local", then jobs are run in-process as a single map

and reduce task.

</description>

</property>

==============================================

#vi hdfs-site.xml

<property>

<name>dfs.replication</name>

<value>1</value>

<description>Default block replication.

The actual number of replications can be specified when the file is created.

The default is used if replication is not specified in create time.

</description>

</property>

==============================================================

This completes the configuration of Hadoop Configuration files.

Create the neccessary directory structure:

$ cd /usr/local

$ sudo tar xzf hadoop-1.0.3.tar.gz

$ sudo mv hadoop-1.0.3 hadoop

$ sudo chown -R hadoop:hadoop hadoop

==============================================================

$cd /usr/local/hadoop/conf

$sudo vi masters

Add this entry only as:

localhost

$sudo vi slaves

Add this entry only as:

localhost

===============================================

Setting up JAVA Environment:

Ensure you are logged in as hadoop user.

$pwd

/home/hadoop

$vi .bashrc

export HADOOP_HOME=/usr/local/hadoop

export JAVA_HOME=/home/hadoop/software/java/jdk1.6.0_45

unalias fs &> /dev/null

alias fs="hadoop fs"

unalias hls &> /dev/null

alias hls="fs -ls"

export PATH=$PATH:$HADOOP_HOME/bin

save the file.

Ensure the following command is working:

$bash

$echo $JAVA_HOME

$echo $JAVA_HOME

Both should show the correct entry as shown in .bashrc

Open the file : /usr/local/hadoop/bin/hadoop-env.sh

Make a one line entry:

export JAVA_HOME=/home/hadoop/software/java/jdk1.6.0_45

save it.

Run bash command simply.

Formatting HDFS:

#/usr/local/hadoop/bin/hadoop namenode -format

It will show like this:

10/05/08 16:59:56 INFO namenode.NameNode: STARTUP_MSG:

/************************************************************

STARTUP_MSG: Starting NameNode

STARTUP_MSG: host = ubuntu/127.0.1.1

STARTUP_MSG: args = [-format]

STARTUP_MSG: version = 0.20.2

STARTUP_MSG: build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-0.20 -r 911707; compiled by 'chrisdo' on Fri Feb 19 08:07:34 UTC 2010

************************************************************/

10/05/08 16:59:56 INFO namenode.FSNamesystem: fsOwner=hduser,hadoop

10/05/08 16:59:56 INFO namenode.FSNamesystem: supergroup=supergroup

10/05/08 16:59:56 INFO namenode.FSNamesystem: isPermissionEnabled=true

10/05/08 16:59:56 INFO common.Storage: Image file of size 96 saved in 0 seconds.

10/05/08 16:59:57 INFO common.Storage: Storage directory .../hadoop-hduser/dfs/name has been successfully formatted.

10/05/08 16:59:57 INFO namenode.NameNode: SHUTDOWN_MSG:

/************************************************************

SHUTDOWN_MSG: Shutting down NameNode at ubuntu/127.0.1.1

************************************************************

=================================================================

Enable Passwordless SSH:

$ssh-keygen -t rsa -P ""

It will ask for something and say yes..

$cat /home/hadoop/.ssh/id_rsa.pub >> /home/hadoop/.ssh/authorized_keys

$ssh localhost

Now you can ssh to localhost machine itself without password.

====================================================================

Now lets start the complete Hadoop service through:

/usr/local/hadoop/bin/start-all.sh

It will start namenode and datanode on the same machine..

====================================================================

Now lets load the data into Hadoop Cluster.

$/usr/local/hadoop/bin/hadoop namenode -format

Esnure you start start-all.sh service before running the below command.

$sudo /usr/local/hadoop/bin/start-all.sh

Lets copy the data as shown below:

$cd /usr/local/hadoop

$sudo bin/hadoop dfs -copyFromLocal /home/hadoop/software/Downloads/1Record /user/hadoop/1

This successfully loads a file called 1 into your HDFS filesystem..

How to verify?

Run this command:

$cd /usr/local/hadoop

$sudo bin/hadoop dfs -ls /

It will show a new directory called /user being created.

Now Lets analyze the dataset:

$cd /usr/local/hadoop

$bin/hadoop jar hadoop-examples-1.0.3.jar wordcount /user/hadoop/1 /user/hduser/1-results

Now you can open http://<ip>:50030 to see the hadoop jobs

Ensure you open this link in VM itself as http://localhost:50030.

if networking works in between host and VM you can successfully open it on host itself.
