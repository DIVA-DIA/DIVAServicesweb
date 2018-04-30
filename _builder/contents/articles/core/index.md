---
title: Core Principles
template: article.jade
---

# Introduction

DIVAServices is a RESTFul Web Service framework for Document Image Analysis (DIA) methods.
The current entry API endpoint is available http://divaservices.unifr.ch/api/v2.

In simple terms this means the framwork provides access to DIA methods through basic HTTP GET and POST requests. This also means that DIVAServices provides NO user-interface but it provides an API for accessing and executing DIA methods in the cloud.

If you would like to experiment with the available methods we provide [DIVAServices-Spotlight](/DIVAServicesweb/articles/spotlight)


## RESTful Principle

The RESTful principle is explained in the visualization below. In DIVAServices GET requests can be used to access information and POST requests can be used to execute methods.

![RESTful Principle](/DIVAServicesweb/articles/core/rest.png)
([Source](https://crunchify.com/how-to-create-restful-java-client-with-jersey-client-example/))

As an example `GET http://divaservices.unifr.ch/api/v2` will list all available methods in JSON as below:
``` JSON
[
  {
    "name": "Otsu Binarization",
    "description": "Otsu Binarization",
    "type": "binarization",
    "url": "http://divaservices.unifr.ch/api/v2/binarization/otsubinarization/1"
  },
  {
    "name": "HOOSC Feature Extraction",
    "description": "This code extracts Histogram of Orientation Shape Context (HOOSC) local shape descriptor from an input hieroglyph image.",
    "type": "imageprocessing",
    "url": "http://divaservices.unifr.ch/api/v2/imageprocessing/hooscfeatureextraction/1"
  }
]
```
An in-depth explanation of the API endpoints is available [here](/DIVAServiceswebarticles/api).