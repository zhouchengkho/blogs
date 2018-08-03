## Learning Docker

### Intro
So what's all the fuss about docker? Why is it beneficial than the old school way? I will talk about this with my hands-on experience these days.

### What is Docker?
Docker is a container manager. (straight forward answer)
I used to love straight forward answer, every time somebody wants to walk me thru a background story before answering the question it pisses me off. Then I found out I am doing the exact same thing... Because I realized a lot of questions are not simply yes or no, or give a definition and that's it.
So in order to know more about Docker, lets walk through a couple of concepts (at least how I understand it).
* image
    * image is a light-weight linux machine with minimum dependency installed.
    * for example, to build an image with only python on it: write a Dockerfile as following:

```
FROM python:2.7-slim
```


    
* container
    * a container is an image running certain commands. Docker Doc has a pretty awesome python | Flask web app tutorial, check that out.
    * so to my understanding, an image contains the lib, project we need, and a container will be running a process based on those data.


### Dockerfile
Dockerfile is how we construct an image. We build based on an base image, then work from there and add a bunch of custom lib or settings. Here is an example of building a python flask app with oracle connection enabled. Details will be explained in the comment.


```
# Dockerfile

# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# resolve oracle dependency
# this is the crazy part... anything to do with oracle can be a bit of a headache
# so assume we have oracle instantclient folder under out host project ./lib/
# then this is for moving that folder and creating soft links
RUN mkdir -p /opt/oracle && mv /app/lib/instantclient_12_2/ /opt/oracle/ \
&& ln -s /opt/oracle/instantclient_12_2/ibclntsh.so.12.1 /opt/oracle/instantclient_12_2/ibclntsh.so \
&& ln -s /opt/oracle/instantclient_12_2/libocci.so.12.1 /opt/oracle/instantclient_12_2/libocci.so \
&& apt update && apt install libaio1

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV LD_LIBRARY_PATH /opt/oracle/instantclient_12_2/

# Run app when the container launches
ENTRYPOINT ["python", "index.py"]
```



### docker-compose.yml
TBD


### Wrap Up
Docker is pretty awesome if you don't want to mess up server's config/env and have a lot of free space.

