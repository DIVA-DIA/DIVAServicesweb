---
title: Data on DIVAServices
template: article.jade
---

# Handling Data on DIVAServices
All data that is stored on DIVAServices is open, and can be accessed by anyone.
We currently don't have plans to build in additional security features in order to hide data.

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


### File content inside the request


### Uploading multiple files

## Adding files to a collection


