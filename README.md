# Bitnami Project - Jenkins Pipeline

The intention of this README is to explain the architecture implemented to resolve the following assignment:

1. Deploy a Jenkins instance and install any plugin that may be needed to address the next
bullet points.

2. Obtain periodically the list of containerized applications (The list of applications we
maintain can be obtained by listing our GitHub repositories named using the pattern
“bitnami-docker-{{application-name}}”.) , and for each of them, create a
parameterized pipeline job. If possible, one of the parameter could be a dropdown
selector with the different version options that the application supports (see
“bitnami-docker-node” for an example).

3. Each individual pipeline job should be able to, at least, clone the repository and perform
some actions on it to check the published application is correct. The
docker-compose.yml file can be used for that purpose. A simple test for checking the
solution is running properly would be ok.

4. (Optional) In Bitnami we have hundreds of apps so we need to run everything in parallel
and to release them as soon as possible. Configure Jenkins slaves that are able to run
Docker for testing the applications in parallel.

## Getting Started - Item (1)

In order to accomplish item 1. A Jenkins instance was installed on an EC2 Instance under the following URL:

http://ec2-18-216-206-98.us-east-2.compute.amazonaws.com:8080

Login Details:
  - user     : admin
  - password : bitnamiadmin

Installed Version: CloudBees Jenkins Enterprise 2.89.1.7-rolling. I have installed a 14-day trial Cloudbees Jenkins Enterprise because of the following reason:

In order to discover Bitnami containerized applications, I installed the GitHub Organization Folder plugin which requires a jenkinsfile (declarative pipeline) commited on each Github repository. As I do not have permissions to commit this file to Bitnami repositories, I installed the Cloudbees Jenkins Enterprise version that allows me to specify an unique jenkinsfile, hosted on a remote repository, that will be used among each and every discovered repository. 

During installation, I installed default recommended plugins plus some specific ones needed to resolve the project assignment. Installed plugins can be found under the following URL.

http://ec2-18-216-206-98.us-east-2.compute.amazonaws.com:8080/pluginManager/installed

Under the EC2 Linux, I installed the following tools:

- Docker
- Git
- Docker Compose
- Java 8

## Configuration - Item (2)

In order to obtain periodically the list of containerized applications and automatically create a job per each Repository master branch, I installed the GitHub Organization Plugin, which allows Jenkins to discover repositories / branches based on regular expressions. This plugin will monitor the Bitnami GitHub organization every day.

In order to create a parameter selector containing every different version (releases) that the application supports, I have installed the Git Parameter Plugin which allows us to assign a git tag or revision number as parameter in Parametrized builds.

As a suggestion, if mayor releases are created with a different naming convention, this parameter could be configured based on a regular expresion that matches only these mayor releases. For this demo, I have configured it to retrieve tags based on the following regular expression *-r*)

Jobs can be found under:

http://ec2-18-216-206-98.us-east-2.compute.amazonaws.com:8080/job/Bitnami/

If you want to see the way I configured the GitHub Organization Folder Plugin, you can go to 

http://ec2-18-216-206-98.us-east-2.compute.amazonaws.com:8080/job/Bitnami/configure

### Configuration Parameters Overview:

- Repository Regular Expression: bitnami-docker*

- Branch Regular Expression : master

- Marker File (File that needs to be present in the repository in order to be consider a buildable repository): docker-compose.yml

- Declarative Pipeline Script: Points to a jenkinsfile commited under this repository . https://github.com/mairadanielaferrari/bitnami-
project.git. File Name: jenkinsfile

## Pipeline - Item (3)

Each time that a person want to verify a master branch, should go to the corresponding job, click on the "build with parameters" link on the left, select the version that wants to verify  and click on build. (Take into account that the version that you want to build should contain a docker-composer.yml file)

Pipeline Explanation:

The declarative pipeline that is used to build each repository is commited on this repository under the name jenkinsfile.

https://github.com/mairadanielaferrari/bitnami-project/blob/master/jenkinsfile

It is composed by 3 stages

### Checkout 
   
   Checkout master branch based on the specific version selected in the parameter Version,

### Validate App

runs docker-composer.yml up -d and verify if each container was loaded propertly by analizing it's .State.ExitCode.

### Run Platform Specific Tests

Simulates Testing Runs for Linux and Windows in parallel under "Windows Testing" and "Linux Testing" stages. Each specific platform test run on a different jenkins slave provisioned by a configured Amazon Cloud. (for testing purposes, both windows and linux slave provision a linux ec2.)

## Slaves - Item (4)

In order to provison slaves for testing, under the main jenkins configuration page Cloud Section, I have configured an EC2 Cloud (bitnami-test-cloud) that will be used by jenkins to provision corresponding slaves while running the pipeline.

http://ec2-18-216-206-98.us-east-2.compute.amazonaws.com:8080/configure

I have configured 3 slaves under the cloud.

- docker-slave where runs the ValidateApp stage
- windows-testing where will run windows tests.
- linux-testing where will run linux tests.

### Final Considerations.

Jenkins and Slaves are running on EC2 free tier instances, which are configured to use really low resources (memory and disk). Processing many jobs at a time might take time. and sometimes when docker-compose up downloads docker images, slaves may run out of space causing a build failure.

If you have any doubt or concern, please send me an email to mferrari@gmail.com

## Authors
Maira Daniela Ferrari
