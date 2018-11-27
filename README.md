Hello world, again. H2O is already relatively easy to launch, all the user needs is a compatiable Java version but now that level of difficulty is reduce to nil. Jeff, our DevOps engineer, presented me with a Docker container for H2O making shipping H2O possible regardless of your environment setup. You can now launch H2O in an isolated environment or container which is great for testing in addition to running H2O smoothly. Launching H2O with Docker has actually been done by one of our users but below is our template which of course can be modified to include R or Spark. The following has been tested and tried on Linux and MacOS. On a Mac, Docker creates a lightweight virtualization that abstracts a process into a container. On a linux, it uses LXC (Linux Containers) to share resources.
For those that never used Docker before you can read all about what Docker is, how to install, and how to launch the application here.
Launch a H2O Instance using Docker
The steps include:

Installation of Docker on Mac or Linux OS
Creating and modifying Dockerfile
Build Docker image from Dockerfile
Run Docker build
Launch H2O
Access H2O from the web browser or R
Video Tutorial
![Video](https://youtu.be/eOg5dj5RnAA)

Walkthrough
Prerequisites

Linux kernal version 3.8+
or

Mac OS X 10.6+
Note: Older linux kernal versions are known to cause kernal panics and to break Docker, there are ways around it but attempt at your own risk.
You can check the version of your kernal by running uname -r in your terminal. The following walk-through has been tested on a Mac OS X 10.10.1.
Step 1 – Install and Launch Docker

Mac Installation
Ubuntu Installation
Other OS Installations
Step 2 – Create or Download Dockerfile
First create a folder on the Host OS to host your Dockerfile by running:
mkdir -p /data/h2o-mirzakhani
Then either download or create a Dockerfile. The Dockerfile is essentially a build recipe that will be used to build our container.
Download and use our Dockerfile template by running:

cd /data/h2o-mirzakhani
$ wget https://blog.h2o.ai/wp-content/uploads/2015/01/Dockerfile
The Dockerfile will:

Pull the base image which is ubuntu 14.04 and run updates.
Install Java 7
Fetch and download h2o mirzakhani from H2O’s S3 repository
Expose port 54321 and 54322 in preparation for launching H2O on those ports
Step 3 – Build Docker image from Dockerfile
From the /data/h2o-mirzakhani directory, run:

$ docker build -t="h2o.ai/mirzakhani" .
This process can take a few minutes as it assembles all the necessary parts to the image.
Step 4 – Run Docker Build
On a Mac, it is necessary to use the argument -p 54321:54321 to expressly map the port 54321. This is redundant on a linux.

$ docker run -it -p 54321:54321 h2o.ai/mirzakhani
Step 5 – Launch H2O
Step into the /opt directory and launch H2O. Adjust -Xmx to the memory you want to allocate to the H2O instance. By default H2O launches on port 54321.

$ cd /opt
$ java -Xmx1g -jar h2o.jar
Step 6 – Access H2O from the web browser or R

On a linux, when h2o finishes launching you can copy and paste the IP address and port of the H2O instance. In the following example that would be 172.17.0.5:54321.
03:58:25.963 main      INFO WATER: Cloud of size 1 formed [/172.17.0.5:54321 (00:00:00.000)]
If it is running on a Mac however you will need to find the IP address of the Docker’s network that bridges to your Host OS. To do this open a new terminal (not a bash for your container) and run boot2docker ip.
$ boot2docker ip
192.168.59.103
Once you have the IP address, point your browser to the specified ip address and port. In R you can access the instance by installing the latest version of the H2O R package and running:

library(h2o)
dockerH2O <- h2o.init(ip = "192.168.59.103", port = 54321)
Let us know how installation went for you or if you run into any errors or problems, in the comment section below.
