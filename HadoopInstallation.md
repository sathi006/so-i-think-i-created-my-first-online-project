Required Software

Required software for Linux and Windows include:

> Java^TM 1.6.x, preferably from Sun, must be installed.
> ssh must be installed and sshd must be running to use the Hadoop scripts that manage remote Hadoop daemons.

Additional requirements for Windows include:

> Cygwin - Required for shell support in addition to the required software above.

Installing Software

If your cluster doesn't have the requisite software you will need to install it.

For example on Ubuntu Linux:

> $ sudo apt-get install ssh
> $ sudo apt-get install rsync

On Windows, if you did not install the required software when you installed cygwin, start the cygwin installer and select the packages:

> openssh - the Net category

Download

To get a Hadoop distribution, download a recent stable release from one of the Apache Download Mirrors.
Prepare to Start the Hadoop Cluster

Unpack the downloaded Hadoop distribution. In the distribution, edit the file conf/hadoop-env.sh to define at least JAVA\_HOME to be the root of your Java installation.

Try the following command:

> $ bin/hadoop

This will display the usage documentation for the hadoop script.

Now you are ready to start your Hadoop cluster in one of the three supported modes:

> Local (Standalone) Mode
> Pseudo-Distributed Mode
> Fully-Distributed Mode

Standalone Operation

By default, Hadoop is configured to run in a non-distributed mode, as a single Java process. This is useful for debugging.

The following example copies the unpacked conf directory to use as input and then finds and displays every match of the given regular expression. Output is written to the given output directory.

> $ mkdir input
> $ cp conf/**.xml input
> $ bin/hadoop jar hadoop-**-examples.jar grep input output 'dfs[a-z.]+'
> $ cat output/

Pseudo-Distributed Operation

Hadoop can also be run on a single-node in a pseudo-distributed mode where each Hadoop daemon runs in a separate Java process.
Configuration

Use the following:

conf/core-site.xml:



&lt;configuration&gt;


> 

&lt;property&gt;


> > 

&lt;name&gt;

fs.defaultFS

&lt;/name&gt;


> > 

&lt;value&gt;

hdfs://localhost:9000

&lt;/value&gt;



> 

&lt;/property&gt;




&lt;/configuration&gt;



conf/hdfs-site.xml:



&lt;configuration&gt;


> 

&lt;property&gt;


> > 

&lt;name&gt;

dfs.replication

&lt;/name&gt;


> > 

&lt;value&gt;

1

&lt;/value&gt;



> 

&lt;/property&gt;




&lt;/configuration&gt;



conf/mapred-site.xml:



&lt;configuration&gt;


> 

&lt;property&gt;


> > 

&lt;name&gt;

mapred.job.tracker

&lt;/name&gt;


> > 

&lt;value&gt;

localhost:9001

&lt;/value&gt;



> 

&lt;/property&gt;




&lt;/configuration&gt;



Setup passphraseless ssh

Now check that you can ssh to the localhost without a passphrase:

> $ ssh localhost

If you cannot ssh to localhost without a passphrase, execute the following commands:

> $ ssh-keygen -t dsa -P '' -f ~/.ssh/id\_dsa
> $ cat ~/.ssh/id\_dsa.pub >> ~/.ssh/authorized\_keys

Execution

Format a new distributed-filesystem:

> $ bin/hadoop namenode -format

Start the hadoop daemons:

> $ bin/start-all.sh

The hadoop daemon log output is written to the $HADOOP\_LOG\_DIR directory (defaults to $HADOOP\_PREFIX/logs).

Browse the web interface for the NameNode and the JobTracker; by default they are available at:

> NameNode - http://localhost:50070/
> JobTracker - http://localhost:50030/

Copy the input files into the distributed filesystem:

> $ bin/hadoop fs -put conf input

Run some of the examples provided:

> $ bin/hadoop jar hadoop-**-examples.jar grep input output 'dfs[a-z.]+'**

Examine the output files:

Copy the output files from the distributed filesystem to the local filesytem and examine them:

> $ bin/hadoop fs -get output output
> $ cat output/

or

View the output files on the distributed filesystem:

> $ bin/hadoop fs -cat output/

When you're done, stop the daemons with:

> $ bin/stop-all.sh

Fully-Distributed Operation


How To Configure Hosts file

http://hayesdavis.net/2008/06/14/running-hadoop-on-windows/

http://www.cygwin.com/

Eclipse Plugin for Juno:

http://yiyujia.blogspot.com/2012/10/eclipse-mapreduce-plugin-build-for.html