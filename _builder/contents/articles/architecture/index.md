---
title: Architecture
template: article.jade
---

# Architecture
The current architecture of DIVAServices is visualized in the Figure below:

![DIVAServices Architecture](/DIVAServicesweb/articles/architecture/architecture.png)

So we can see DIVAServices is comprised of three different elements:
 - DIVAServices (the core framework)
 - A Docker server
 - A file storage server

Each of these elements is described below.

## DIVAServices (the core framework)
The core framework is programmed in [TypeScript](https://www.typescriptlang.org/) a super script of JavaScript with added types.

DIVAServices is as a web server and handles all incoming requests and serves back the information.

DIVAServices uses [JSON](https://www.json.org/) as data transmission format.

### Handling GET Requests
In case of an incoming GET request, all that the framework needs to do is look up the specific information and send it back to the client.

### Handling POST Requests
A POST request is always used to execute a particular method. This can be achieved by calling the URL of the method.
DIVAServices parses this incoming request, perform some sanity checking and execute a Docker Container of the specific method.
Once the method finished, DIVAServices will retrieve the results and provide them to the client.

#### Asynchronous Execution
Because some methods can take a long time until they are executed, DIVAServices handles the communication for executing methods in an asynchronous way.

Whenever a method execution is started, the client immediately receives a JSON response as follows:
```JSON
{
	"results": [
		{
			"resultLink": "http://divaservices.unifr.ch/api/v2/results/victoriousabandonedbassethound/data_0/data_0.json"
		}
    ],
    ...
	"status": "done",
	"statusCode": 202
}
```
The client that started the execution has then to consistenly poll the `results:resultLink` URL(s). Once an execution is finished this GET request will respond with a JSON object like the following:

``` JSON
{
  "output": [
    ... specific outputs of the method
  ],
  "status": "done",
  "resultLink": "http://divaservices.unifr.ch/api/v2/results/smugquintessentialbaiji/data_0/data_0.json",
  "collectionName": "smugquintessentialbaiji"
}
```
The `status` field can be used as an identifier to check against if the execution is finished or not.

## Docker Server
DIVAServices makes use of [Docker](https://docker.io) in order to execute the methods.
For each image a Docker Image is created that contains the method including all the binaries and libraries required for it to run.

A tutorial for how to create such a Docker image is available [here](/DIVAServicesweb/articles/provide-your-method)

### Common Workflow Language
In order to actually execute the Docker images (and to prepare for provding pipeline support on DIVAServices) we make use of the [Common Workflow Language](http://www.commonwl.org/). 


## File Storage
The last piece in the architecture is the file storage. We rely on a normal Linux filesystem that is shared between DIVAServices and Docker through [NFS](http://tldp.org/HOWTO/NFS-HOWTO/intro.html).