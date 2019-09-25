# Hosted Feature Service - Services reference

This Postman collection is aimed at facilitating the use and knowledge of the ArcGIS REST Endpoints serving as a complement to the documentation provided in
[https://developers.arcgis.com/rest/services-reference/hosted-feature-service.htm](https://developers.arcgis.com/rest/services-reference/hosted-feature-service.htm)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Getting started](#getting-started)
- [Available SDKs](#available-sdks)
- [Working with Databases (Feature Services)](#working-with-databases-feature-services)
  - [How to check if a database exists](#how-to-check-if-a-database-exists)
  - [How to create an empty database](#how-to-create-an-empty-database)
  - [How to get database info](#how-to-get-database-info)
  - [How to manage database security](#how-to-manage-database-security)
- [Working with tables (Feature Layers)](#working-with-tables-feature-layers)
  - [How to manage tables](#how-to-manage-tables)
    - [Create a table](#create-a-table)
    - [Add fields to a table](#add-fields-to-a-table)
    - [Remove fields from a table](#remove-fields-from-a-table)
    - [Remove a table](#remove-a-table)
  - [How to manage records in a table](#how-to-manage-records-in-a-table)
    - [Query records](#query-records)
    - [Add records](#add-records)
    - [Update records](#update-records)
    - [Delete records](#delete-records)
    - [Add attachments to a record](#add-attachments-to-a-record)
  - [How to manager layer views (filter / extend tables)](#how-to-manager-layer-views-filter--extend-tables)
    - [Create a layer view](#create-a-layer-view)
    - [Delete a layer view](#delete-a-layer-view)
  - [How to manage a relationship](#how-to-manage-a-relationship)
    - [Create a relationship](#create-a-relationship)
    - [Query a relationship](#query-a-relationship)
    - [Delete a relationship](#delete-a-relationship)
- [Other recommended resources](#other-recommended-resources)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Getting started

First if you have not already done so download Postman and import this collection and the environment variables.

Now you will need to [set up some variables](https://developers.onelogin.com/api-docs/1/getting-started/postman-collections) like: `username` and `password`.

This documentation is written is by describing common use cases, so if you want to create an empty database:

1. Go to [the respective section](#how-to-create-an-empty-database)
2. Read the documentation
3. Go to Postman and find the `Endpoint name` and test it,

If you have any other questions please use the [repository issues](https://github.com/esri-es/ArcGIS-REST-API/issues).

## Available SDKs

Here we are going to show you how to work directly with the REST API, but if you are a JavaScript or a Python developer you might want to take a look to the available SDKs:

* [ArcGIS API for Python](https://github.com/Esri/arcgis-python-api)
* [ArcGIS REST JS](https://esri.github.io/arcgis-rest-js/api/)

> **Note**: *none of them have support to every capability of the REST API, that's why we still think it's worth learning how to use it directly.*

## Working with Databases (Feature Services)

### How to check if a database exists

> **Endpoint name**: [\<root-url\>/portals/\<instance\>/isServiceNameAvailable](https://developers.arcgis.com/rest/users-groups-and-items/check-service-name.htm)<br>
> **Example**:
[https://www.arcgis.com/sharing/rest/portals/rF1wdZICHfgsvter/isServiceNameAvailable?name=Testing_purposes_POSTMAN_Collection&f=json&token=...&type=Feature%20Service](https://www.arcgis.com/sharing/rest/portals/rF1wdZICHfgsvter/isServiceNameAvailable?name=Testing_purposes_POSTMAN_Collection&f=json&token=...&type=Feature20Service)

### How to create an empty database

> **Endpoint name**: [\<root-url\>/content/users/\<username\>/createService](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm) <br>
> **Example**: https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/createService

### How to get database info

> **Endpoint name**: [\<catalog-url\>/\<serviceName\>/FeatureServer](https://developers.arcgis.com/rest/services-reference/feature-service.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json](
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json)

### How to manage database security

*PENDING*

## Working with tables (Feature Layers)

### How to manage tables

#### Create a table

> **Endpoint name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/addToDefinition](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-service-.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition)

geometry of point type

#### Add fields to a table

*PENDING*

#### Remove fields from a table

*PENDING*

#### Remove a table

> **Endpoint name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/deleteFromDefinition](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-service-.htm)<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition)

### How to manage records in a table

*PENDING*

#### Query records

*PENDING*

Resources:

* [Query a feature layer](https://developers.arcgis.com/labs/rest/query-a-feature-layer/) (tutorial)
* [Using the ArcGIS REST Query Page](https://www.youtube.com/watch?v=LsYgtjkm69Y) (video)

#### Add records

*PENDING*

Resources:

* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)

#### Update records

*PENDING*

Resources:

* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)

#### Delete records

*PENDING*

Resources:

* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)

#### Add attachments to a record

*PENDING*

### How to manager layer views (filter / extend tables)

#### Create a layer view

*PENDING*

#### Delete a layer view

*PENDING*

### How to manage a relationship

#### Create a relationship

*PENDING*

#### Query a relationship

*PENDING*

#### Delete a relationship

*PENDING*

## Other recommended resources

Videos:

* [Interacting with Hosted Feature Layers through the REST API](https://www.youtube.com/watch?v=uJvZ8MJA0t4)
* [Node.js and browser applications with ArcGIS REST JS](https://www.youtube.com/watch?v=omPiCTXZ0U8)

Other written resources:

* [Esri Open Vision > Open Specs > ArcGIS REST API](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/arcgis-rest-api/)
