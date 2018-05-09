---
title: Data on DIVAServices
template: article.jade
---

# Handling Data on DIVAServices
All data that is stored on DIVAServices is open, and can be accessed by anyone.
We currently don't have plans to build in additional security features in order to hide data.

**Experiment it live**
- There is a IPython Notebook available on [Google Colab]()

**Important:** This means, as defined in the [Terms of Services](https://github.com/lunactic/DIVAServices/blob/master/TOS.md) data provided to DIVAServices has to be licensed under a [Creative Commons](https://creativecommons.org/) (at least CC-BY-SA)

**Important:** As DIVAServices is **NOT** a data store, we do not guarantee that data provided to DIVAServices is stored long-term and data can be deleted without notice.

## Data Organization
Data on DIVAServices is organized in `collections`. Each collection serves a bucket for a number of individual files that belong together.

## Accessing existing Data
As described in the [API](/DIVAServicesweb/articles/api) Tutorial, all collection information is available through the `/collections` API.

A GET request to [`/collections`](http://divaservices.unifr.ch/api/v2/collections) provides information on all currently available collections. The response looks as follows:

``` JSON
{
  "collections": [
    {
      "collection": {
        "name": "irritatingflickeringpitbull",
        "url": "http://divaservices.unifr.ch/api/v2/collections/irritatingflickeringpitbull"
      }
    },
    {
      "collection": {
        "name": "dopeyscholarlykentrosaurus",
        "url": "http://divaservices.unifr.ch/api/v2/collections/dopeyscholarlykentrosaurus"
      }
    },
    {
      "collection": {
        "name": "brownwigglyasianconstablebutterfly",
        "url": "http://divaservices.unifr.ch/api/v2/collections/brownwigglyasianconstablebutterfly"
      }
    }
  ]
}
```
The `collections` array contains the list of existing collections.

A GET request to the `url` of a collection, provides additional information about the collection, as well as the files that are available:

``` JSON
{
  "statusCode": 200,
  "statusMessage": "Collection is available",
  "percentage": 100,
  "totalFiles": 1,
  "files": [
    {
      "file": {
        "url": "http://divaservices.unifr.ch/api/v2/files/irritatingflickeringpitbull/original/inverseImage.jpg",
        "identifier": "irritatingflickeringpitbull/inverseImage.jpg"
      }
    }
  ]
}
```
We plan on providing additional information about collections in the future (e.g. tags to specify the contents of a collection)
## DIVAServices file identifiers
Each file on DIVAServices has an `identifier` this `identifier` is used to refer to data elements when executing methods.

**Example**

The above described file could be used in the POST request to e.g. a binarization method as follows:

``` JSON
{
  "parameters":{},
	"data":[
    {
      "inputImage": "irritatingflickeringpitbull/inverseImage.jpg"
    }
  ]
}
```

## Creating a new collection
New collections are created by creating a POST request to `/collections`.
Collections of files can be created in different ways as described below.

**Note**

The `name` parameter is optional when creating a new collection. If provided, this `name` will be used as identifier for the new collection. Otherwise a randomly generated name is used.

### Files from a URL
The simples way for transferring files to DIVAServices if they are accessible through a URL. The request to create a new collection then looks as follows:

``` JSON
{
  "name": "YourCollectionName",
  "files":[
    {
      "type":"url",
      "value":"http://www.e-codices.unifr.ch/loris/bnf/bnf-lat11641/bnf-lat11641_001r.jp2/full/pct:25/0/default.jpg",
      "name":"bnf-lat11641_001r"
    }
  ]
}
```

This will create a collection named `YourCollectionName`.

In case a collection with the same name exists already, an error is returned with the following message:
``` JSON
{
  "errorType" : "DuplicateCollectionError",
  "message" : "A collection with the name: YourCollectionName already exists."
}
```

### File content inside the request
Files can be provided mainly in two different ways:

#### Providing as URL
The simples way of providing files was shown in the example above. Whenever possible files should be referenced from publicly accessible URLs. DIVAServices will then download the files from there and store them on the File Storage.

When referencing from a URL the JSON object for a file needs to fullfill the following requirements:
 - `type` needs to be set to `url`
 - `value` needs to point to the publicly accessible URL where the file can be downloaded

#### Providing as base64 encoded string
In case files can not be provided as URLs, files can be provided using base64 encoding. Base64 encoding can be achieved in many programming languages, and in the Google Colab an example is provided in Python.

When using base64 encoding, the JSON object for a file needs to fullfill the following requirements:
- `type` needs to be set to `base64`
- `value` needs to be the base64 encoded string of the file.

### Uploading multiple files
Furthermore it is possible to upload more than one file at the time. For this one simply puts more than one JSON object into the `files` array:

```JSON
{
  "name":"testCollection",
  "files":[
    {
      "type":"url",
      "value":"http://www.e-codices.unifr.ch/loris/ubb/ubb-A-II-0004/ubb-A-II-0004_0002r.jp2/full/pct:25/0/default.jpg",
      "name":"ubb-A-II-0004_0002r"
    },
    {
      "type":"url",
      "value":"http://www.e-codices.unifr.ch/loris/ubb/ubb-A-II-0004/ubb-A-II-0004_0004r.jp2/full/pct:25/0/default.jpg",
      "name":"ubb-A-II-0004_0004r"
    }
  ]
}
```

**Note**: If you want to create a collection with multiple files from base64 encoded images, be aware that the size of the request can not exceed 200MB. If you want to add more files than this, create the collection with a single file first and then add more files to the existing collection as explained below.

## Adding files to a collection
Files can be added to a collection using a PUT request to `/collections/:collectionName`.
The structure of the `request-body` is the same as above and files can be added either as `url` or `base64` encoded.


## Remove a collection
A collection can be deleted with a DELETE request to `/collections/:collectionName`.
