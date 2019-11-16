



## Gateway Setup
https://freighttrust.com
https://t.me/freighttrust
https://github.com/freight-trust
https://twitter.com/freighttrustnet

### admin@freighttrust.com

##### please read CLA and DCO 

###### licensed under bsd-3-clause & other open source licenses including gpl2.



Installing Master Node Client


## Setting up client 

```
General availability client: RHEL/CentOS 7.4-7.7
disallowed: other distros (no ubuntu). 

Reference Build Vanilla 
http://linuxsoft.cern.ch/cern/centos/7/rt/CentOS-RT.repo

```
#### 

 - [ ] java
 - [ ] node.js
 - [ ] virtual box
 - [ ] docker
 - [ ] lxc
 - [ ] rust
 - [ ] mosh
 - [ ] 
 - [ ] wget/curl/zsh
 - [ ] &&
 - [ ] &&&

prep terminal
ssh into instance on aws / gcp



##### 1. prep terminal 

$ sudo dnf install mosh

```
$ cd /etc/yum.repos.d
wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
```
##### 2. Install docker & docker machine
install  docker-ce & docker-machine
```
base=https://raw.githubusercontent.com/docker/machine/v0.16.0
for i in docker-machine-prompt.bash docker-machine-wrapper.bash docker-machine.bash
do
sudo wget "$base/contrib/completion/bash/${i}" -P /etc/bash_completion.d
done 
```
⚠️ issue with cgroup while installing docker? try: 
```docker cgroup issue:
$ sudo yum install cgroup-lite
```

recommended fusion/baseimage

[https://hub.docker.com/r/phusion/baseimage/](https://hub.docker.com/r/phusion/baseimage/)
##### 3. install node/npm
```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

##### 4. install  oracle java 13.0.1 
```
$ yum install -y https://maidenlane.s3.amazonaws.com/jdk-13.0.1_linux-x64_bin.rpm(https://maidenlane.s3.amazonaws.com/jdk-13.0.1_linux-x64_bin.rpm)
```
##### 6. download besu client base image 
```
$ docker pull besu:madienlane &&  
```
##### 5. start client
```
$ bin/besu
```



##### 6. Import private key from initial validator genesis 
```
$ mv 0x1234ABCD.key /user/ftn#/besu/config
```
##### 7. Import private key from initial validator genesis 
```
$ ./inspect.sh
$ ./gateway.sh
```
..




## teeDai - trusted execution environment (inter)side (change)chain

##### Creating the sterlingOS 
a performant bare metal configuration for  Intel x86 SGX clustering

Hardware working on:

PowerEdge R230 BiOS 2.4+ 1270 R230 Xeon 

Currently SGX1 coverage, as no cloud provider has SGX2 out-of-the-box + bios support.

##### Trusted Setup for zK-SNARK/SGX "
##### 8. Intel SGX Sidechain Enclave ☢️
#####   End User Notice 
 Adjusting these settings can result in an unstable system. These are
 root only and assumes root knows what they are doing.

Most notable:

 ⚠️ very small values in sched_rt_period_us can result in an unstable
   system when the period is smaller than either the available hrtimer
   resolution, or the time it takes to handle the budget refresh itself.

 ⚠️ very small values in sched_rt_runtime_us can result in an unstable
   system when the runtime is so small the system has difficulty making
   forward progress (NOTE: the migration thread and kstopmachine both
   are real-time processes).

Realtime scheduling is all about determinism, a group has to be able to rely on
the amount of bandwidth (eg. CPU time) being constant. In order to schedule
multiple groups of realtime tasks, each group must be assigned a fixed portion
of the CPU time available.

#### Network Wide Settings

2.1 System wide settings
------------------------
Make sure you have

[SGX Hardware Support](https://github.com/ayeks/SGX-hardware)
[Intel Product Specifications (check to see if your specific make supports](https://ark.intel.com/content/www/us/en/ark.html)

``Intel SGX driver installed and /dev/isgx works``
``gdb 7.11.1``
`development libraries needed:`
`$ yum install libcgroup libcgroup-tools libcurl4-openssl-dev libssl-dev`

``` [baiduxlabs/sgx-rust](https://hub.docker.com/r/baiduxlab/sgx-rust) ```
>⛔️ **rustup** for install, MUST NOT use package managers, i.e. yum, rpm, apt


The system wide settings are configured under the /proc virtual file system:

/proc/sys/kernel/sched_rt_period_us:
  The scheduling period that is equivalent to 100% CPU bandwidth

/proc/sys/kernel/sched_rt_runtime_us:
  A global limit on how much time realtime scheduling may use.  Even without
  CONFIG_RT_GROUP_SCHED enabled, this will limit time reserved to realtime
  processes. With CONFIG_RT_GROUP_SCHED it signifies the total bandwidth
  available to all realtime groups.

  * Time is specified in us because the interface is s32. This gives an
    operating range from 1us to about 35 minutes.
  * sched_rt_period_us takes values from 1 to INT_MAX.
  * sched_rt_runtime_us takes values from -1 to (INT_MAX - 1).
  * A run time of -1 specifies runtime == period, ie. no limit.
requires [https://github.com/mesalock-linux](https://github.com/mesalock-linux)



#### Disable irqbalance daemon
```
# service irqbalance status
irqbalance (pid PID) is running...
```

**if irqbalance daemon is running, disable..**
```
# service irqbalance stop
Stopping irqbalance: [ OK ]
```
Use chkconfig to ensure that irqbalance does not restart on boot.
```
# chkconfig irqbalance off
```

Binding Processes to CPUs using the taskset utility


#### SGX Driver
```
$ curl --output /tmp/driver.bin https://download.01.org/intel-sgx/linux-2.3.1/ubuntu18.04/sgx_linux_x64_driver_4d69b9c.bin
/usr/bin/env bash /tmp/driver.bin
depmod
modprobe isgx
```
verify install: ```dmesg``` should show ``` Intel SGX Driver v0.11``` install was successful 


#### configure libsecp256k

configure the libsecp256k1 library by going into 

```
$ cd src/trusted/libs/


```
### aesmd daemon 
```
$ docker run --rm -it -v /run/aesmd:/run/aesmd --device /dev/isgx <AWS ECR LINK>
```


#### Building Client 

(I)   Protocol
(II)  Servlet 
(III) Enclavelet


Notes:
Maven pom.xml config

define JVM configuration via ${maven.projectBasedir}/.mvn/jvm.config

``` 
 -Xmx2048m -Xms1024m -XX:MaxPermSize=512m -Djava.awt.headless=true

Maven pom.xml

MAVEN_OPTS=-XX:MaxRAM=3572m -Xmx1g
-XX:MaxRAM=3572m -Xmx8g


___

> (c)[freight trust & clearing corporation](https://freighttrust.com/)  
> licensed under bsd-3 clause & other open source licenses. 