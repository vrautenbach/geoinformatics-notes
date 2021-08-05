# Getting started with Geoserver

## Overview

[GeoServer](http://geoserver.org) is an open source software server written in Java that allows users to share and edit geospatial data. Designed for interoperability, it publishes data from any major spatial data source using open standards. GeoServer is the reference implementation of the Open Geospatial Consortium \(OGC\) Web Feature Service \(WFS\) and Web Coverage Service \(WCS\) standards, as well as a high performance certified compliant Web Map Service \(WMS\). GeoServer forms a core component of the Geospatial Web.

This page provides resources and some guidelines on how to get started with PostGIS. Note that this page provides a basic overview and is not meant to be step-by-step instructions. You have to adapt information available online for your purposes to complete the task.

## Web Map Service and Web Feature Service

Web services are independent, modular applications that can be published, located and dynamically invoked across the web.

A web service is a program created with a specific purpose and to be able to use the web service, the user requires little to no knowledge about the implementation of the web service, but must be familiar with the interface.

The two main web services that you will use is web map service \(WMS\) and web feature service \(WFS\). WMS is used to generate an image of your data, whereas WFS is used to access the actual data. Below is examples of both WMS and WFS.

### WMS example

![WMS example](https://github.com/vrautenbach/geoinformatics-notes/blob/master/images/WMSexample.png)

Read more here about the WMS implementation in GeoServer, [https://docs.geoserver.org/latest/en/user/services/wms/index.html](https://docs.geoserver.org/latest/en/user/services/wms/index.html)

### WFS example

![WFS example](https://github.com/vrautenbach/geoinformatics-notes/blob/master/images/WFSexample.png)

Read more here about the WFS implementation in GeoServer, [https://docs.geoserver.org/latest/en/user/services/wfs/index.html](https://docs.geoserver.org/latest/en/user/services/wfs/index.html)

## Loading a PostGIS layer into GeoServer

The [GeoServer User Guide](https://docs.geoserver.org/latest/en/user/index.html) is very detailed and explains the process that needs to be followed very well.

To load data from PostGIS into GeoServer, review these instructions, [https://docs.geoserver.org/latest/en/user/gettingstarted/postgis-quickstart/index.html](https://docs.geoserver.org/latest/en/user/gettingstarted/postgis-quickstart/index.html)

