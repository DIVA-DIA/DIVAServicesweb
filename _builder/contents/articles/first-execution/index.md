---
title: Execute your First method
template: article.jade
---

# Execute your first method on DIVAServices
As discussed in the [Architecture](/DIVAServicesweb/articles/architecture) article, methods on DIVAServices are executed using HTTP POST request.
In this Tutorial we want to execute a first simple method on DIVAServices.
This method requires one input file and produces one output file.

For this Tutorial we will use the [Grayification Method](http://divaservices.unifr.ch/api/v2/enhancement/grayification/1).

## Interpreting the GET request information
A GET request (with the browser or any other application) to the URL of the method (see link above), will return the following information in JSON:

```JSON
{
  "general": {
    "name": "Grayification",
    "description": "Image Grayification",
    "developer": "Manuel Bouillon",
    "affiliation": "University of Fribourg",
    "email": "manuel.bouillon@unifr.ch",
    "author": "Manuel Bouillon",
    "type": "enhancement",
    "license": "Other",
    "ownsCopyright": "1",
    "expectedRuntime": 14.321428571428571,
    "executions": 28
  },
  "input": [
    {
      "file": {
        "name": "inputImage",
        "description": "The input image to transform",
        "options": {
          "required": true,
          "mimeTypes": {
            "allowed": [
              "image/jpeg",
              "image/png",
              "image/tiff"
            ],
            "default": "image/jpeg"
          }
        },
        "userdefined": true
      }
    },
    {
      "outputFolder": {
        "userdefined": false
      }
    }
  ],
  "output": [
    {
      "file": {
        "name": "grayImage",
        "type": "image",
        "description": "Generated Grayscale Image",
        "options": {
          "mimeTypes": {
            "allowed": [
              "image/jpeg",
              "image/png"
            ],
            "default": "image/jpeg"
          },
          "colorspace": "grayscale",
          "visualization": true
        }
      }
    }
  ]
}
```
In order to execute the method, two parts are important, the `input` and the `output` array.
These specify what kind of inputs the method requires and what kind of outputs it produces.
For more information there is the article on [Inputs and Outputs](/DIVAServicesweb/articles/inputs-and-outputs)

For this tutorial we need to know that the method requires the following inputs:
- An input `file` specified in `inputImage` that needs to be either a JPEG, a PNG, or a TIFF.
- An `outputFolder` that will automatically be provided by DIVAServices (as `userdefined` is `false`).

And the method will produce as output:
- A `file` that is an `image`, which is either a JPEG, a PNG, or a TIFF, that could be used by a third-party system as a `visualization`.

## Generating the POST request
With the information from the GET request we can put together the POST request for executing the method.

For the `inputImage` we need the *DIVAServices identifier* for the image to binarize. Details for getting this are describe in the Tutorial on [Data Management](/DIVAServicesweb/articles/first-execution).
For this Tutorial we use the identifier `testCollection/ubb-A-II-0004_0002r.jpeg` which points to [this](http://divaservices.unifr.ch/api/v2/files/testCollection/original/ubb-A-II-0004_0002r.jpeg) image.
And we need to remember, that information about data elements goes into the `data` part of the `request-body` of the POST request.

So the POST request will look like this:

```JSON
{
    "parameters":{},
    "data":[
        {
            "inputImage": "testCollection/ubb-A-II-0004_0002r.jpeg"
        }
    ]
}
```
This returns the immediate response:
``` JSON
{
    "results": [
        {
            "resultLink": "http://divaservices.unifr.ch/api/v2/results/genuinetubbycero/data_0/data_0.json"
        }
    ],
    "collection": "genuinetubbycero",
    "resultLink": "http://divaservices.unifr.ch/api/v2/results/genuinetubbycero.json"
}
```
And when polling for the `resultLink` inside the `results` array you will get the following result of the method:

```JSON
{
  "output": [
    {
      "file": {
        "name": "grayImage",
        "type": "image",
        "description": "Generated Grayscale Image",
        "options": {
          "colorspace": "grayscale",
          "visualization": true
        },
        "mime-type": "image/jpeg",
        "url": "http://divaservices.unifr.ch/api/v2/results/genuinetubbycero/data_0/grayImage.jpeg"
      }
    },
    {
      "file": {
        "mime-type": "text/plain",
        "type": "log",
        "url": "http://divaservices.unifr.ch/api/v2/results/genuinetubbycero/data_0/logFile.txt",
        "name": "logFile.txt",
        "options": {
          "visualization": false
        }
      }
    }
  ],
  "status": "done",
  "resultLink": "http://divaservices.unifr.ch/api/v2/results/genuinetubbycero/data_0/data_0.json",
  "collectionName": "genuinetubbycero"
}
```

In the `output` array there are two `file` entries:
 - `grayImage`: The output image that is the result of the method.
 - `logFile`: The logfile of the method.

Each with a `url` to the location where the file can be downloaded from.

## Examples
We provide the execution example for the above explanation in different programming languages.

### Python
``` Python
import requests

url = "http://divaservices.unifr.ch/api/v2/enhancement/grayification/1"

payload = " {\"parameters\":{},\"data\":[{\"inputImage\": \"testCollection/ubb-A-II-0004_0002r.jpeg\"}]}"
headers = {'content-type': 'application/json'}

response = requests.request("POST", url, data=payload, headers=headers)
```
And to poll for the result the following method can be used:

``` Python
def pollResult(self, result_link):
    """ Polls for the result of the execution in 1s intervals 
    Arguments:
        result_link {string} -- [the resultLink generated by the POST request that started the execution]
        
    Returns:
        [json] -- [the result of the execution]
    """

    response = json.loads(requests.request("GET", result_link).text)
    while(response['status'] != 'done'):
        if(response['status'] == 'error'):
            sys.stderr.write('Error in executing the request. See the log file at: ' + response['output'][0]['file']['url'])
            sys.exit()
        time.sleep(1)
        response = json.loads(requests.request("GET", result_link).text)
    return response

```
Where `result_link` in this case would be `response['outputs'][0]['resultLink']`.

### Java
```Java
HttpResponse<JSONObject> response = Unirest.post("http://divaservices.unifr.ch/api/v2/enhancement/grayification/1")
  .header("content-type", "application/json")
  .body(" {\"parameters\":{},\"data\":[{\"inputImage\": \"testCollection/ubb-A-II-0004_0002r.jpeg\"}]}")
  .asJSON();
```
A simliar polling strategy can be applied as in Python.


### JavaScript (with JQuery)
``` JavaScript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://divaservices.unifr.ch/api/v2/enhancement/grayification/1",
  "method": "POST",
  "headers": {
    "content-type": "application/json"
  },
  "processData": false,
  "data":"{\"parameters\":{},\"data\":[{\"inputImage\":\"testCollection/ubb-A-II-0004_0002r.jpeg\"}]}"
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
A simliar polling strategy can be applied as in Python.

### Node.js

``` JavaScript
var request = require("request");

var options = { method: 'POST',
  url: 'http://divaservices.unifr.ch/api/v2/enhancement/grayification/1',
  headers: { 'content-type': 'application/json' },
  body: 
   { parameters: {},
     data: [ { inputImage: 'testCollection/ubb-A-II-0004_0002r.jpeg' } ] },
  json: true };

request(options, function (error, response, body) {
  if (error) throw new Error(error);
  console.log(body);
});

```

A simliar polling strategy can be applied as in Python.