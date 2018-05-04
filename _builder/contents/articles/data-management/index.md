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



## Uploading Data

### Uploading a single file

### Uploading multiple files