# Udacity Capstone

## The final cloud computing project

### Project Overview

To reproduce this project, create a new EC2 instance (t2.micro - Ubuntu 18.04 LTS amd64) on AWS.

#### Install AWS CLI

*Follow this guide* <https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html>

**Note:** Make sure you use pip and not apt.

Add your credentials with `$ aws configure`

#### Install Jenkins

```
sudo apt-get update
sudo apt install -y default-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update

sudo apt-get install -y jenkins
```

**Note:** Make sure you allow connections on port 8080.

#### Install Jenkins Plugins

* Install standard plugins
* Install Blue Ocean
* Install Github pipeline for Blue Ocean
* Install Pipeline AWS

#### Add your AWS credentials to Jenkins

* Click on Credentials from the sidebar
* Click on Global, then add your credentials from the sidebar
* Choose AWS credentials from the dropdown
* Enter your AWS keys and click ok

#### Add your Docker credentials to Jenkins

* Follow the same steps as above
* Choose Username with Password instead of AWS credentials

#### Install Docker on your EC2 instance 

*Follow this guide* <https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04>

#### Install dependencies for AWS EKS

*Follow this guide* <https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html>

#### Create two new Github repositories

Create a new project folder called **blue_green_pipeline** and one called **cluster_creation**.

#### Cluster Creation

In the cluster_creation folder, create a Jenkinsfile to use EKS to create a Kubernetes cluster. Also, within the Jenkinsfile create a configuration file to store the information about your cluster. 

*Completed Jenkinsfile* <https://github.com/bgilbank/udacity_capstone_project/blob/master/cluster_creation/Jenkinsfile>

Once your Jenkins file is completed, open Jenkins in your browser and add the new pipeline inside of Blue Ocean (cluster_creation).

If the build is successful, open the AWS console and navigate to EKS. Copy your arn information. You will need this to set your kubectl context.   

#### Blue / Green deployment pipeline

In the blue_green_pipeline folder, create a Jenkinsfile to set up the blue/green deployment stages.

#### Create a Dockerfile

To build a local image on your EC2 instance, you will need to create a basic Dockerfile inside of your blue-green-pipeline folder.

*Completed file* <https://github.com/bgilbank/udacity_capstone_project/blob/master/blue_green_pipeline/Dockerfile>

#### Create Blue and Green Controller files

Create two new files inside of the blue_green_pipeline folder. These files will be **blue_controller.json** and **green_controller.json**.

These are replicationController's, which ensure that the specified number of pod replicas are always up and running. In this case, we only have one specified.

*Completed green_controller.json* <https://github.com/bgilbank/udacity_capstone_project/blob/master/blue_green_pipeline/green_controller.json>

Completed blue_controller.json <https://github.com/bgilbank/udacity_capstone_project/blob/master/blue_green_pipeline/blue_controller.json>

#### Update and configure the Dockerfile to

1. Lint your HTML file
1. Build a local Docker image
1. Push your image to Dockerhub
1. Set Kubectl context 
1. Deploy the Blue container
1. Deploy the Green container
1. Create a service and redirect traffic to the Blue container
1. Wait for user input
1. Update the service to redirect traffic to the Green Container

*Completed Jenkinsfile* <https://github.com/bgilbank/udacity_capstone_project/blob/master/blue_green_pipeline/Jenkinsfile>

Essentially, a blue green deployment runs two identical environments, so that you can test and validate your code before pushing it to a live environment. This approach reduces overall risk by allowing you to switch back to a working environment if one fails. The Kubernetes cluster that was created allows you to do this with the `$ kubectl config use-context` command.  

#### Make the service visible

Inside of the project folder (blue_green_pipeline), create a new file called blue_green_service.json. This file will make the green app externally visible in step 9 of the Jenkinsfile. 

*Completed file* <https://github.com/bgilbank/udacity_capstone_project/blob/master/blue_green_pipeline/blue_green_service.json>

#### Add the Blue Green pipeline repository to Blue Ocean

That's it, if all tests pass!