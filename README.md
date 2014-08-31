wfdb-scaling
============

Improving the scalability of the WFDB Toolbox for Matlab and Octave


Abstract
--------
The PhysioNet WFDB Toolbox for MATLAB and Octave is a collection of functions for reading, writing, and processing physiologic signals and time series used by PhysioBank databases. Using the WFDB Toolbox, researchers have access to over 50 PhysioBank databases consisting of over 3TB of physiologic signals. These signals include ECG, EEG, EMG, fetal ECG, PLETH (PPG), ABP, respiration, and others. 

The WFDB Toolbox provides support for local concurrency; however, it does not currently support distributed computing environments. Consequently, researchers are unable to utilize the additional resources afforded by popular distributed frameworks. The present work improves the scalability of the WFDB Toolbox by adding support for distributed environments. Further, it aims to supply several best-practice examples illustrating the gains that can be realized by researchers leveraging this form of concurrency.


Overview
--------
This project aims to provide simple instruction and examples for researchers familiar with the WFDB Toolbox who are insterested in taking advantage of distributed computing environments. Specifically,

1. deploying an appropriate Amazon EC2 cluster, and
2. running their code on this cluster.


Cluster Deployment
------------------
Deploying and managing a cluster is often a non-trivial task. Therefore, we will be using [StarCluster](http://star.mit.edu/cluster) to quickly and simply create a suitable cluster on Amazon EC2. The following assumes you already have an AWS account, so if you do not, head over to [AWS](http://aws.amazon.com/) and create one.


### Installing StarCluster
Detailled steps can be found in [StarCluster's installation documentation](http://star.mit.edu/cluster/docs/latest/installation.html). 

However, many users can install the project using `pip`:

```sh
pip install StarCluster
```

(Note: Depending on your account permissions, you may have to use `sudo` or contact your system administrator for the above command.)

### Configuring StarCluster
Detailled steps can be found in [StarCluster's quickstart guide](http://star.mit.edu/cluster/docs/latest/quickstart.html). 

However, for a WFDB-specific cluster the following steps should be followed. First, create a config file by running:
```sh
$ starcluster help
StarCluster - (http://star.mit.edu/cluster)
Software Tools for Academics and Researchers (STAR)
Please submit bug reports to starcluster@mit.edu

cli.py:87 - ERROR - config file /home/user/.starcluster/config does not exist

Options:
--------
[1] Show the StarCluster config template
[2] Write config template to /home/user/.starcluster/config
[q] Quit

Please enter your selection:
```
Enter 2 to create a new config file from the template. Second, update the template to correctly specify your `[aws info]`:

```
[aws info]
aws_access_key_id = #your aws access key id here
aws_secret_access_key = #your secret aws access key here
aws_user_id = #your 12-digit aws user id here
```

Third, create a WFDB keypair for accessing the instances that will be created:

```sh
starcluster createkey wfdbkey -o ~/.ssh/wfdbkey.rsa
```

(Note: If this step fails, you may need to ensure that the user specified in the `[aws info]` above has privileges to instantiate new instances. This can be done from the Amazon EC2 dashboard.)

Finally, add WFDB-specific cluster templates from this project's [config](config) to yours:

```sh
curl https://raw.githubusercontent.com/tnaumann/wfdb-scaling/master/config >> ~/.starcluster/config
```

### Deploy it!
To deploy the most basic WFDB-specific cluster run:

```sh
starcluster start -c wfdbcluster mycluster
```

After the cluster has started, you can `ssh` to the master node as root by running:

```sh
starcluster sshmaster mycluster
```

Finally, you can temporarily stop the cluster by running:

```sh
starcluster stop mycluster
```

Or terminate it entirely by running:

```sh
starcluster terminate mycluster
```


Benchmarks
----------
In addition to the `wfdbcluster` cluster template provided, there are also three others that were used for benchmarking:
  * `wfdbcluster-small`: 2 node
  * `wfdbcluster-medium`: 4 node
  * `wfdbcluster-large`: 8 node

Each of the benchmark clusters uses `m1.small` instance types which provide a good balance of performance for cost; however, the could easily be modified to take advantage of more powerful EC2 instance types which would be expected to further improve performance.

