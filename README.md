# Bitnami Project - Jenkins Pipeline

One Paragraph of project description goes here

## Getting Started

The intention of this README is to explain the architecture used to define the following assignment:

Deploy a Jenkins instance and install any plugin that may be needed to address the next
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

### Prerequisites

What things you need to install the software and how to install them

```
Give examples
```

### Installing

A step by step series of examples that tell you have to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
