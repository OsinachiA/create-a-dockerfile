#This script details how to create a dockerfile for a static website. A dockerfile is basically a set of instructions for creating a container. 


# The first step is to specify a base image (The base image comes from dockerhub. On the dockerhub website you can find the base image). You can search the base image in Dockerhub. We’ll use amazon linux as the base image, and latest as the tag. 

FROM amazonlinux:latest

# Next, we’ll use RUN in all capitals and the list the commands that we’ll like to run. This step is for installing dependencies. 

# Install dependencies
#first, update the packages on the system, next, install apache web sever, install wget command, as wget command may not be on the system but is required to download the webfiles from github. After this, we I'll now download the package to unzip the zip folder downloaded using the wget commmand. 
RUN yum update -y && \
    yum install -y httpd && \
    yum search wget && \
    yum install wget -y && \
    yum install unzip -y

# Next, thing is to change my directory to a html directory (change directory) 
RUN cd /var/www/html

# next, we'll use wget to download the webfiles from github
RUN wget https://github.com/azeezsalu/jupiter/archive/refs/heads/main.zip

# Next is to unzip the zipped folder that has been downloaded.
RUN unzip main.zip

# Next, is to copy all the web files into the html directory
RUN cp -r jupiter-main/* /var/www/html/

# remove unwanted folder. The unwanted folders include the zip folder that we downloaded from github and the folder that we unzipped. 
RUN rm -rf jupiter-main main.zip

# The next step focusses on the port that we would want to expose. In this case, we'll be exposing port 80 on the container.
EXPOSE 80

# set the default application that will start when the container start. 
# in this case we'll be using entyrypoint.
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
