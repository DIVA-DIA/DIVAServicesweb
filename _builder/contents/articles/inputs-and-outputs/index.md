---
title: Inputs and Outputs
template: article.jade
---
- [Inputs and Outputs Specification](#inputs-and-outputs-specification)
  - [Inputs](#inputs)
  - [General Information](#general-information)
    - [File](#file)
      - [Using in POST request](#using-in-post-request)
    - [Folder](#folder)
      - [Using in POST request](#using-in-post-request)
    - [Number](#number)
      - [Using in POST request](#using-in-post-request)
    - [Select](#select)
      - [Using in POST request](#using-in-post-request)
    - [OutputFolder](#outputfolder)
    - [Example](#example)
  - [Outputs](#outputs)
    - [File](#file)
      - [Options for a file](#options-for-a-file)
      - [Example](#example)
    - [Folder](#folder)
    - [Other outputs](#other-outputs)

# Inputs and Outputs Specification
DIVAServices can handle a variety of inputs and outputs. In this Tutorial we describe what Inputs and Outputs are available, how they are specified and how to create requests based off of them.

A [JSON-Schema](http://json-schema.org/) is available [here](https://github.com/lunactic/DIVAServices/blob/development/conf/schemas/detailsAlgorithmSchema.json) that describes it.

## Inputs
The following `input` types are available and are described below:
- `file`: A regular file as input
- `folder`: A complete collection as input
- `number`: A numeric input
- `select`: A string input based from a selection
- `outputFolder`: A path to a folder where output data can be stored
- `text`: A string input

## General Information
The inputs needed for a method are specified in the `inputs` array when creating a GET request to a specific method.

For inputs where the `userdefined` value is set to `true`, the client HAS TO provide information in the POST request when calling the method. Inputs where `userdefined` is `false`, DIVAServices will automatically provide information.

When creating the POST requests, all regular parameters (number, select, text, etc) go into the `parameters` object. Values for data elements (file, folder) go into the `data` array.

The `name` of an input is also the `identifier` that HAS TO be used when providing the information. This shows, that for each specified `input` a corresponding entry with the same `name` as key has to be provided in the POST request either in the `parameters` or a `data` object.

**Example**

For the following input specification of a number:
``` JSON
{
    "number": {
        "name": "threshold",
        "description": "threshold for receiving only 'strong' features",
        "options": {
          "required": true,
          "default": 0.000001,
          "min": 0,
          "max": 1,
          "steps": 0.000001
        },
        "userdefined": true
    }
}
```

The client will have to provide the information as follows:

``` JSON
{
    "parameters":{
        "threshold":0.000001,
        ...
    },
    "data":[
        ...
    ]
}
```

### File
The structure for a `file` description looks as follows:
``` JSON
{
    "file": {
        "name": "inputImage",
        "description": "The input image to binarize",
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
    }
```
The following information is specified:
 - `name`: the identifier of this parameter
 - `description`: A short description of the parameter
 - `options:mimeTypes`: Specifies which mimeTypes can be provided to this input
 
#### Using in POST request
`file` Information has to be provided within a `data` object in the POST request. The client can specify which `file` to use by providing its DIVAServices identifier (combination of the name of the collection and the filename).

**Example**

The above specified file could be used in the following way:
``` JSON
{
    "parameters":{},
    "data":[
        {
            "inputImage": "testCollection/ubb-A-II-0004_0002r.jpeg"
        }
    ]
}
```

### Folder
A `folder` specifies to use the contents of a complete collection. The structure of a `folder` specification is as follows:

``` JSON
{
    "folder": {
        "name": "inputData",
        "description": "The input collection (needs to contain the ground truth *.gt.txt, and input text lines *.bin.png files)",
        "options": {
          "required": true
        },
        "userdefined": false
    }
}
```

#### Using in POST request
`folder` information has to be provided within a `data` object in the POST request. The client can specify which `folder` to use by providing the `name` of a collection.

**Example**

``` JSON
{
    "data":[
        {
            "inputData":"ocr_train_example"
        }
    ],
    "parameters":{
        ...
    }
}
```
### Number
A `number` specifies a numeric input. This numeric input can be further specified using `min`, `max` and `steps`. In JSON it is specified as follows:

``` JSON
{
    "number": {
        "name": "lineHeight",
        "description": "the default line height",
        "options": {
            "required": true,
            "default": 46,
            "min": 10,
            "max": 120,
            "steps": 1
        },
        "userdefined": true
    }
}
```
The following information is specified:
- `name`: The identifier of the input
- `description`: A short description of the input
- `options:default`: The default value to use for this input
- `options:min`: The minimal value that can be used for this input
- `options:max`: The maximal value that can be used for this input
- `options:steps`: The stepsize between two possible values

#### Using in POST request
Values for a `number` input have to be provided in the `parameters` object. The key needs to be the `name` of the input parameter.

**Example**

The above described `number` input could be used as follows in a POST request:

```JSON
{
    "data":[
        ...
    ],
    "parameters":{
        "lineHeight":46
    }
}
```

### Select
A `select` input specifies a selection of various variables that can be selected. In JSON it is specified as follows:

``` JSON
{
    "select": {
        "name": "detector",
        "description": "The Interest Point detector to use",
        "options": {
          "required": true,
          "values": [
            "Harris",
            "Hessian",
            "Laplace",
            "Quadrature",
            "Ridge"
          ],
          "default": "Harris"
        },
        "userdefined": true
    }
}
```
#### Using in POST request
Values for a `select` input have to be provided in the `parameters` object. The key needs to be the `select` of the input parameter.

**Example**

The above specified `select` could be used as follows in a POST request:

``` JSON
{
    "parameters":{
        "detector":"Harris",
        ...
    },
    "data":[
        ...
    ]
}
```
### OutputFolder
The `outputFolder` specifies a folder to the method where it can store its outputs. The client does not need to provide any information for this value as it is automatically provided by DIVAServices.
### Example
The following is a complete example of a method specification provided by DIVAServices.
The method specifies the following parameters:
 - `inputImage`: A `file` input parameer
 - `outputFolder`: An `outputFolder` input parameter
 - `threshold`, `maxFeaturesPerScale`, `blurSigma`, `numScales`, `numOctaves`: `number` input parameters
 - `detector`: A `select` input parameter

``` JSON
{
  "general": {
      ...
  },
  "input": [
    {
      "file": {
        "name": "inputImage",
        "description": "The input image to segment",
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
    },
    {
      "number": {
        "name": "threshold",
        "description": "threshold for receiving only 'strong' features",
        "options": {
          "required": true,
          "default": 0.000001,
          "min": 0,
          "max": 1,
          "steps": 0.000001
        },
        "userdefined": true
      }
    },
    {
      "number": {
        "name": "maxFeaturesPerScale",
        "description": "Maximum features per scale",
        "options": {
          "required": true,
          "default": -1,
          "min": -1,
          "max": 1500,
          "steps": 1
        },
        "userdefined": true
      }
    },
    {
      "select": {
        "name": "detector",
        "description": "The Interest Point detector to use",
        "options": {
          "required": true,
          "values": [
            "Harris",
            "Hessian",
            "Laplace",
            "Quadrature",
            "Ridge"
          ],
          "default": 0
        },
        "userdefined": true
      }
    },
    {
      "number": {
        "name": "blurSigma",
        "description": "Amount of initial blur applied to the image",
        "options": {
          "required": true,
          "default": 1,
          "steps": 0.1
        },
        "userdefined": true
      }
    },
    {
      "number": {
        "name": "numScales",
        "description": "Number of scales per octave",
        "options": {
          "required": true,
          "default": 5,
          "min": 2,
          "max": 6,
          "steps": 1
        },
        "userdefined": true
      }
    },
    {
      "number": {
        "name": "numOctaves",
        "description": "number of octaves computed.  Each consecutive octave is composed of numScales images which have half the resolution of the previous.",
        "options": {
          "required": true,
          "default": 3,
          "min": 2,
          "max": 5,
          "steps": 1
        },
        "userdefined": true
      }
    }
  ],
  "output": [
   ...
  ]
}
```

## Outputs
The following 'output' type(s) are produce by DIVAServices and are described below:

- `file`: An output file that is generated by the method. This can be any sort of file (image, xml, json, csv, txt, etc.). `options` are available to specify the output file in more details.
-  `folder`: An output folder that contains multiple files. 

**Note**: As we are in the process of specifying server-side workflows on DIVAServices, this list might change in the future.

### File
An output `file` can represent any arbitrary file that will be produced by a method running on DIVAServices. The general JSON structure for an output file is as follows:
``` JSON
{
  "file": {
    "name": "otsuBinaryImage",
    "type": "image",
    "description": "Generated Binary Image",
    "options": {
      "mimeTypes": {
        "allowed": [
          "image/jpeg",
          "image/png",
          "image/tiff"
        ],
        "default": "image/jpeg"
      },
      "colorspace": "binary",
      "visualization": true
    }
  }
}
```
The following information is specified:
 - `name`: the identifier of the output file. In the result JSON of the method you will find a `file` entry with the same `name`.
 - `type`:  a specification of the output type (e.g. image, model, etc.) This will be further specified in the future.
 - `description`: A description of the output file.
 - `options` Specifying more options about this output file (see below)

#### Options for a file
the `options` object further specifies the output file.
The following information will always be available:
- `mimeTypes`: An object specifying which [MIME types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) can be produced (in the `allowed` array) and which MIME type should be assumed as a `default`.
- `visualization`: specifies whether or not this output file could be used by a third-party program to be used as a visualization of the methods results.

Additionaly for specific `type`s of output files we envision additional information in the options, e.g:

for the `image` type:
 - `colorspace`: specifying the colorspace of the output image
 - `channels`: providing information about number of channels

We plan to work on this in the near future to provide a more exhaustive list of `option`s.

#### Example
The following is a specification for an output `file` of a method:
``` JSON
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
```
This will generate the following output in the `output` array when the method is executed:

```JSON
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
}
```
The information is preserved but a URL is made available where the resulting file can be downloaded from.

### Folder
A `folder` output represents a collection of multiple output files that belong together.
The complete specification for this is currently still being worked out.

### Other outputs
If your method produces outputs in a different way, feel free to get in touch with us, either by mail or creating an issue on our [Github Repository](https://github.com/lunactic/DIVAServices/issues).

