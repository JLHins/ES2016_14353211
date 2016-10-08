# DOL

------

##1 Description
The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of applications onto the multiprocessor SHAPES architecture platform. The DOL consists of basically three parts:

> * **DOL Application Programming Interface:** The DOL defines a set of computation and communication routines that enable the programming of distributed, parallel applications for the SHAPES platform. Using these routines, application programmers can write programs without having detailed knowledge about the underlying architecture. In fact, these routines are subject to further refinement in the hardware dependent software (HdS) layer.
> * **DOL Functional Simulation:** To provide programmers a possibility to test their applications, a functional simulation framework has been developed. Besides functional verification of applications, this framework is used to obtain performance parameters at the application level.
> * **DOL Mapping Optimization:** The goal of the DOL mapping optimization is to compute a set of optimal mappings of an application onto the SHAPES architecture platform. In a first step, XML based specification formats have been defined that allow to describe the application and the architecture at an abstract level. Still, all the information necessary to obtain accurate performance estimates is contained.



------

## 2 Installation

###2.1 Environment
ubuntu 14.04 64Bits VMware

### 2.2 Preparation
	sudo apt-get update
	sudo apt-get install ant
 	sudo apt-get install openjdk-7-jdk
	sudo apt-get install unzip
	sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
    sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
### 2.3 Unzip

    mkdir dol
    unzip dol_ethz.zip -d dol
    tar -zxvf systemc-2.3.1.tgz
### 2.4 Compile systemc
    cd systemc-2.3.1
    mkdir objdir
    cd objdir
    ../configure CXX=g++ --disable-async-updates
    sudo make install
    
### 2.5 Compile DOL
    pwd ##the location will be used later
    cd ../dol
    sudo vim build_zip.xml
```xml
##Change "YYY" below into "location" before
##Change "lib-linux" to "lib-linux64" if your machine is 64 bits
<property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
```
    ant -f build_zip.xml all    ##show "build successful" if successful
    
###2.6 Run an example
    cd build/bin/main
    ant -f runexample.xml -Dnumber=1

    
##3 Summary
#####1.Update environment before experiment is important.
#####2.Sometimes we can't download from official site,use image website can help a lot.



