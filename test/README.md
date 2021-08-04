Moving your app to the cloud with Kubernetes
Part 1 - How to create a Docker app image to use for deployment on Kubernetes

In this tutorial you will learn how to create a Docker app image you will later use to deploy your app on Kubernetes.
We will use ExampleApp as our example throughout this tutorial. You can use it for testing purposes.

Step 1
If you do not have Docker installed, install it from the official website. We will need Docker for building a docker image on your machine.
Step 2
In the command line interface on your machine, run these commands to create the necessary directory and subdirectories:

mkdir quickstart_docker
mkdir quickstart_docker/application
mkdir quickstart_docker/docker
mkdir quickstart_docker/docker/application


Here is what we will use each directory and subdirectory for:

quickstart_docker/ # Main project directory
├──application/      # App file with code goes here
└──docker/           # Docker information goes here
   └──application/    # Dockerfile with build instructions goes here

Step 3
Create your test application file for ExampleApp - application.py - in the quickstart_docker/application subdirectory. Here is the example code the application.py file contains:

import http.server
import socketserver

PORT = 8000

Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer(("", PORT), Handler)

print("serving at port", PORT)
httpd.serve_forever()


The application requires an app environment. We will need to run python scripts, and that would require an operating system with its dependencies. No need to start from scratch, we will refer to the Docker Hub in the Dockerfile (on Step 4) to define the custom environment we will use in the Docker container.
Step 4
Create your Dockerfile using a code editor of your choice. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.

For the example app image, your file should contain the following instructions:

# Use base image from the registry
FROM python:3.5

# Set the working directory to /app
WORKDIR /app

# Copy the 'application' directory contents into the container at /app
COPY ./application /app

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]


For more details on the Dockerfile requirements, format, and instructions reference Docker’s documentation.
Step 5
In the command line interface on your machine, run the command:

$ docker build . -f-docker/application/Dockerfile -t exampleapp


Using the docker build command you can create an automated build that executes several command-line instructions in succession.
You use the -f flag with the docker build command to point to a Dockerfile anywhere in your file system. You are pointing to your working directory and build context.
You use the -t flag to specify a tag at which to save the new image if the build succeeds. This allows you to find the app image later.

Reference Docker’s documentation on building an image for more detail.

Step 6
Let’s review our image list. In the command line interface on your machine, run the command:
$ docker images


It should return the list of images created on your machine in the following format:
REPOSITORY             TAG             IMAGE ID            CREATED             SIZE
exampleapp             latest          83wse0edc28a        2 seconds ago       153MB
python                 3.6             05sob8636w3f        6 weeks ago         153MB


If your ExampleApp image is showing up on that images list, congratulations, you have created your app image successfully.
If it does not, reference this troubleshooting guide (linking to the guide).

What’s next?
Follow Part 2 of the tutorial to learn how to push your app image into your repository.
(linking to Part 2).
