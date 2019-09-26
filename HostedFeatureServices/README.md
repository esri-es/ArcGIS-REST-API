# Hosted Feature Service - Services reference

We are aware that the documentation from the [ArcGIS REST API](https://developers.arcgis.com/rest/services-reference/get-started-with-the-services-directory.htm) can be overwhelming to new ArcGIS Developers, that why  we created this focused resource.

This resource has been created for them, so it we will focus on explaining how to host data in [ArcGIS Online](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-online/) (the Esri cloud product), although they work in an equivalent way in [ArcGIS Enterprise](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-enterprise/) (the Esri on-premises product).

As far as possible we will try to avoid the technicalities of geographic information systems in order to facilitate understanding.

The [Postman collection included](./Hosted%20Feature%20Service%20-%20ArcGIS.postman_collection.json) is aimed at facilitating the use and understanding of [the official documentation](https://developers.arcgis.com/rest/services-reference/hosted-feature-service.htm) serving as a complement to it.

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Getting started](#getting-started)
- [Available SDKs](#available-sdks)
- [Previous concepts](#previous-concepts)
  - [ArcGIS Online items (geoinformation model)](#arcgis-online-items-geoinformation-model)
  - [ArcGIS Online Architecture](#arcgis-online-architecture)
- [Working with Databases (Feature Services)](#working-with-databases-feature-services)
  - [How to check if a database exists](#how-to-check-if-a-database-exists)
  - [How to create an empty database](#how-to-create-an-empty-database)
  - [How to get database info](#how-to-get-database-info)
  - [How to change database info](#how-to-change-database-info)
  - [How to manage database options](#how-to-manage-database-options)
    - [Visibility](#visibility)
    - [Edition, manage indexes, cache control and allow export data](#edition-manage-indexes-cache-control-and-allow-export-data)
- [Working with tables (Feature Layers and Tables)](#working-with-tables-feature-layers-and-tables)
  - [How to manage tables](#how-to-manage-tables)
    - [Create a table](#create-a-table)
      - [Example of point layer](#example-of-point-layer)
      - [Example of polyline layer](#example-of-polyline-layer)
      - [Example of polygon layer](#example-of-polygon-layer)
      - [Example of table](#example-of-table)
    - [Change a table name](#change-a-table-name)
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

---

## Getting started

First if of all, if you haven't an ArcGIS Online account we encourage you to create a [free developer account](http://developers.arcgis.com/sign-up).

Now, please download [Postman](https://www.getpostman.com/downloads/) and import [this collection](./Hosted%20Feature%20Service%20-%20ArcGIS.postman_collection.json) and [the environment variables](./Hosted%20Feature%20Services%20ArcGIS.postman_environment.json).

Next [set up some variables](https://developers.onelogin.com/api-docs/1/getting-started/postman-collections) like: `username` and `password`.

You are ready to go, feel free to explore. This documentation is written by describing common use cases, so if you want to create an empty database:

1. Go to [the respective section](#how-to-create-an-empty-database)
2. Read the documentation
3. Go to Postman and find the `Endpoint name` and test it,

If you have any other questions please use the [repository issues](https://github.com/esri-es/ArcGIS-REST-API/issues).

## Available SDKs

Here we are going to show you how to work directly with the REST API, but if you are a JavaScript or a Python developer you might want to take a look to the available SDKs:

* [ArcGIS API for Python](https://github.com/Esri/arcgis-python-api)
* [ArcGIS REST JS](https://esri.github.io/arcgis-rest-js/api/)

> **Note**: *none of them have support to every capability of the REST API, that's why we still think it's worth learning how to use it directly.*

## Previous concepts

When working with hosted feature services is better to be familiar with the ArcGIS [geoinformation model](https://doc.arcgis.com/en/arcgis-online/reference/geo-info.htm) and the [ArcGIS Online Architecture](https://docs.google.com/drawings/d/e/2PACX-1vRbMbrSXgE8fGdsIz5RBgGNpixoPPgJ6swlk9vT3lUyW8cOffUxmb3Oludm7yF44BzwRoTPtZ5jvwGx/pub?w=3870&amp;h=2405) in order to better understand why the endpoints are designed the way they are.

### ArcGIS Online items (ArcGIS geoinformation model)

> **Note**: items ArcGIS Enterprise works the same way

An `Item` is the core unit of the geoinformation model. To better understand the concept of an `Item`, it is necessary to bear in mind that ArcGIS Online is designed from its base to be used for organizations with multiple users (not just developers), and so that any user of an organization (especially those without programming skills) can create, share, find and manage with other users all kinds of geolocalized information::

* Data through: databases and static files (CSV, GeoJSON, Excels, ...)
* Static files: PDFs, ZIPs, etc
* Astonishing 2D and 3D maps (possible thank to ths ArcGIS map specifications like [Web Maps](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/web-map/) and [Web Scenes](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/web-map/))
* Configuration files for apps
* And much more

Bearing this in mind, when we create a free developer account we are assigned an organizational account that can only have a single user, so there are some capabilities such as sharing an item with a group that will not be very useful, as there are no more users in our organization.

> The only exception is that we want to group several public items in a public group for reasons of organization and visibility of the items.

So an `Item` is just a JSON plain object stored in ArcGIS Online which contains information about the resource (database, static file, etc) but also permissions and other properties.

As a first approach, imagine each user in ArcGIS Online has a item table associated with it, something like this:

[![Item table](https://user-images.githubusercontent.com/826965/65668230-a0fde280-e041-11e9-8d8b-8347483291be.png)](https://user-images.githubusercontent.com/826965/65668230-a0fde280-e041-11e9-8d8b-8347483291be.png)

And this is a very basic example of the JSON that describes what it looks like a **Hosted Feature Service item** (some properties might differ based on the item type):

```js
{
	"id": "8729cf95b89c44c283324210bbb1af3a",
	"owner": "awesome_arcgis",
	"created": 1569481074000,
	"isOrgItem": true,
	"modified": 1569483820000,
	"guid": null,
	"name": "NewFeatureService",
	"title": "NewFeatureService",
	"type": "Feature Service",
	"typeKeywords": ["ArcGIS Server", "Data", "Feature Access", "Feature Service", "Service", "Hosted Service"],
	"description": null,
	"tags": [],
	"snippet": null,
	"thumbnail": "thumbnail/ago_downloaded.gif",
	"documentation": null,
	"extent": [],
	"categories": [],
	"spatialReference": null,
	"accessInformation": null,
	"licenseInfo": null,
	"culture": "",
	"properties": null,
	"url": "https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer",
	"proxyFilter": null,
	"access": "private",
	"size": 0,
	"appCategories": [],
	"industries": [],
	"languages": [],
	"largeThumbnail": null,
	"banner": null,
	"screenshots": [],
	"listed": false,
	"ownerFolder": null,
	"protected": false,
	"commentsEnabled": true,
	"numComments": 0,
	"numRatings": 0,
	"avgRating": 0,
	"numViews": 2,
	"itemControl": "admin",
	"scoreCompleteness": 33,
	"groupDesignations": null
}
```

Additional resources:

* [Supported item types](https://doc.arcgis.com/en/arcgis-online/reference/supported-items.htm)
* [Items and item types](https://developers.arcgis.com/rest/users-groups-and-items/items-and-item-types.htm) (developer documentation)
* Web GIS and geoinformation model (https://www.youtube.com/watch?v=MSJetLXD-Cw&list=PLoptan2utx16J4O_JpRqYiTxpATG1QKjD&index=2) (video in Spanish)
* [ArcGIS Platform basic concepts](https://youtu.be/5WLmzvtwJqg?t=532) (video in Spanish)

### ArcGIS Online Architecture

ArcGIS online is **much more** than hosted feature services, that's we we encourage you to take a look to this [ArcGIS Online resource page](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-online/) and this simplified diagram of the ArcGIS Online Architecture to have better understanding on how it works:

[![ArcGIS Online Architecture](https://esri-es.github.io/awesome-arcgis/arcgis/products/product-thumbnails/arcgis-online.png)](https://docs.google.com/drawings/d/e/2PACX-1vRbMbrSXgE8fGdsIz5RBgGNpixoPPgJ6swlk9vT3lUyW8cOffUxmb3Oludm7yF44BzwRoTPtZ5jvwGx/pub?w=3870&amp;h=2405)

## Working with Databases (Feature Services)

### How to check if a database exists

> **Endpoint name**: [\<root-url\>/portals/\<instance\>/isServiceNameAvailable](https://developers.arcgis.com/rest/users-groups-and-items/check-service-name.htm)<br>
> **Example**:
[https://www.arcgis.com/sharing/rest/portals/rF1wdZICHfgsvter/isServiceNameAvailable?name=Testing_purposes_POSTMAN_Collection&f=json&token=...&type=Feature%20Service](https://www.arcgis.com/sharing/rest/portals/rF1wdZICHfgsvter/isServiceNameAvailable?name=Testing_purposes_POSTMAN_Collection&f=json&token=...&type=Feature20Service)

Checks whether a given service name and type are available for publishing a new service. true indicates that the name and type is not found in the organization's services and is available for publishing. false means the requested name and type are not available.

**Read**: [full documentation](https://developers.arcgis.com/rest/users-groups-and-items/check-service-name.htm)

### How to create an empty database

> **Endpoint name**: [\<root-url\>/content/users/\<username\>/createService](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm) <br>
> **Example**: https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/createService

The Create Service operation allows users to create a hosted feature service. You can use the API to create an empty hosted feature service from a feature service metadata JSON.

This request will create an [Item](#arcgis-online-items-geoinformation-model) and an empty hosted feature service:

* **Item**: as we saw it main responsibilities are provide:
    * **Common information to any item type for [it details page](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)** like: item type, item title, description, owner, creation date, thumbnail, tags, number of views, comments, ratings, ... | Example: [public Item page](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1).
    * **Security properties**: visibility, delete protection flag, ...
    * But also information more specific information (which will depend on the type of item), for example for the a Hosted Feature Service the URL of the REST Endpoint.
* **Hosted Feature Service**: it main responsabilities are provide: 
    * **Database info**: pagination size, supported query formats, spatial reference, initial extent, ...
    * **Layer and Tables**:
    	* **Layer and table info**: geometry type, fields information, information about how to draw the markers, display de popups, etc.
    	* **Layer and table data**: mechanism to manage de data
    * **Security and performance settings**: who can edit what, manage indexes, cache control and allow export data

The API REST endpoints to manage the Item and the Hosted Feature Service are differents:

* To manage the item we will use something like: 
	* `https://www.arcgis.com/sharing/rest/content/users/<username>/operation`
	* `https://www.arcgis.com/sharing/rest/content/users/<username>/items/<itemId>/update`
* To manage the Hosted Feature Service we will use something like: 
	* `https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/<serviceName>/FeatureServer/addToDefinition`
	* `https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/<serviceName>/FeatureServer/<layerId>/addToDefinition`

**Read**: [full documentation](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm)

### How to get database info

> **Endpoint name**: [\<catalog-url\>/\<serviceName\>/FeatureServer](https://developers.arcgis.com/rest/services-reference/feature-service.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json](
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json)

This resource provides basic information about the feature service, including the feature layers and tables that it contains, the service description, and so on.

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/feature-service.htm)

### How to change database info

> **Endpoint name**: [\<root-url\>/content/users/\<username\>/items/\<itemId\>/update](https://developers.arcgis.com/rest/services-reference/feature-service.htm)<br>
> **Example**: [
https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update](
https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update)



**Read**: [full documentation](https://developers.arcgis.com/rest/users-groups-and-items/update-item.htm)

### How to manage database options

#### Visibility

> **Endpoint name**: [\<root-url\>/content/users/\<username\>/shareItems](https://developers.arcgis.com/rest/users-groups-and-items/share-items.htm)<br>
> **Example**: [https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/shareItems](https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/shareItems)

This operation allows to share a database (including all contained tables) with:

* Everyone, in which case they are publicly accessible
* With any user in an ArcGIS organization
* With a list of user groups within ArcGIS.

**Read**: [full documentation](https://developers.arcgis.com/rest/users-groups-and-items/share-items.htm).

#### Edition, manage indexes, cache control and allow export data

> **Endpoint**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/updateDefinition](https://developers.arcgis.com/rest/services-reference/update-definition-feature-service-.htm)
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/updateDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/updateDefinition)

This operation allows us to change who is able to edit the service, who can do what, if we want to keep track of who created and updated records and when, manage spatial indexes, enable cache (CDN), and manage if export data is publicly for everyone. The result of this operation is a response indicating success or failure with error code and description.

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/update-definition-feature-service-.htm)

## Working with tables (Feature Layers and Tables)

First we are going to explain the different ways to store information in an ArcGIS hosted (cloud) database.

* **A table**: is just a regular table with rows and columns.
* **A Feature Layer**: is a little bit more, it is a table that contains a spatial geometry type associated to each row (Point, Line or Polygon) and metadata information about the map extent of the data.

Let take a look to the differences of the REST endpoint interfaces:

* **Table**: [Service description page](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer/3) | [Query page](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer/3/query)
* **Feature layer** (Point type): [Service description page](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer/0) | [Query page](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer/0/query)

As you may notice, a feature layer provide additional capabilities related to spatial operations, datum  transformations, etc.

So, if you are planning to store spatial information **we strongly encourage you** to use feature layers.

> **Important**: please note that a feature layer cannot contain records with different geometry types simultaneously. You will have to split them in different layers.

### How to manage tables

#### Create a table

> **Endpoint name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/addToDefinition](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-service-.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition)

This operation allows us to add a table to a database (`<serviceName>` param). This  The result of this operation is a response indicating success or failure with error code and description.

In the `addToDefinition` param you have to define which layer type you want to create.

* [Example of point layer](#example-of-point-layer)
* [Example of polyline layer](#example-of-polyline-layer)
* [Example of polygon layer](#example-of-polygon-layer)
* [Example of standalone table](#example-of-standalone-table)

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-service-.htm).

##### Example of point layer

Property `geometryType` needs to be assigned to `esriGeometryPoint`.

```js
{
	"layers": [{
		"currentVersion": 10.51,
		"id": 0,
		"name": "Point layer",
		"type": "Feature Layer",
		"displayField": "",
		"description": "",
		"copyrightText": "",
		"defaultVisibility": true,
		"editingInfo": {
			"lastEditDate": null
		},
		"relationships": [],
		"isDataVersioned": false,
		"supportsAppend": true,
		"supportsCalculate": true,
		"supportsTruncate": true,
		"supportsAttachmentsByUploadId": true,
		"supportsAttachmentsResizing": true,
		"supportsRollbackOnFailureParameter": true,
		"supportsStatistics": true,
		"supportsAdvancedQueries": true,
		"supportsValidateSql": true,
		"supportsCoordinatesQuantization": true,
		"supportsApplyEditsWithGlobalIds": false,
		"advancedQueryCapabilities": {
			"supportsPagination": true,
			"supportsPaginationOnAggregatedQueries": true,
			"supportsQueryRelatedPagination": true,
			"supportsQueryWithDistance": true,
			"supportsReturningQueryExtent": true,
			"supportsStatistics": true,
			"supportsOrderBy": true,
			"supportsDistinct": true,
			"supportsQueryWithResultType": true,
			"supportsSqlExpression": true,
			"supportsAdvancedQueryRelated": true,
			"supportsCountDistinct": true,
			"supportsLod": true,
			"supportsReturningGeometryCentroid": false,
			"supportsQueryWithDatumTransformation": true,
			"supportsHavingClause": true,
			"supportsOutFieldSQLExpression": true
		},
		"useStandardizedQueries": true,
		"geometryType": "esriGeometryPoint",
		"minScale": 0,
		"maxScale": 0,
		"extent": {
			"xmin": -20037508.342788905,
			"ymin": -8175201.3721496435,
			"xmax": -10018754.171394452,
			"ymax": 12175461.54272524,
			"spatialReference": {
				"wkid": 102100
			}
		},
		"drawingInfo": {
			"renderer": {
				"type": "simple",
				"symbol": {
					"type": "esriPMS",
					"url": "RedSphere.png",
					"imageData": "iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAACXBIWXMAAA7DAAAOwwHHb6hkAAAAGXRFWHRTb2Z0d2FyZQBQYWludC5ORVQgdjMuNS4xTuc4+QAAB3VJREFUeF7tmPlTlEcexnve94U5mANQbgQSbgiHXHINlxpRIBpRI6wHorLERUmIisKCQWM8cqigESVQS1Kx1piNi4mW2YpbcZONrilE140RCTcy3DDAcL/zbJP8CYPDL+9Ufau7uqb7eZ7P+/a8PS8hwkcgIBAQCAgEBAICAYGAQEAgIBAQCAgEBAICAYGAQEAgIBAQCDx/AoowKXFMUhD3lQrioZaQRVRS+fxl51eBTZUTdZ41U1Rox13/0JF9csGJ05Qv4jSz/YPWohtvLmSKN5iTGGqTm1+rc6weICOBRbZs1UVnrv87T1PUeovxyNsUP9P6n5cpHtCxu24cbrmwKLdj+osWiqrVKhI0xzbmZ7m1SpJ+1pFpvE2DPvGTomOxAoNLLKGLscZYvB10cbYYjrJCb7A5mrxleOBqim+cWJRakZY0JfnD/LieI9V1MrKtwokbrAtU4Vm0A3TJnphJD4B+RxD0u0LA7w7FTE4oprOCMbklEGNrfdGf4IqnQTb4wc0MFTYibZqM7JgjO8ZdJkpMln/sKu16pHZGb7IfptIWg389DPp9kcChWODoMuDdBOhL1JgpisbUvghM7AqFbtNiaFP80RLnhbuBdqi0N+1dbUpWGde9gWpuhFi95yL7sS7BA93JAb+Fn8mh4QujgPeTgb9kAZf3Apd2A+fXQ38yHjOHozB1IAJjOSEY2RSIwVUv4dd4X9wJccGHNrJ7CYQ4GGjLeNNfM+dyvgpzQstKf3pbB2A6m97uBRE0/Ergcxr8hyqg7hrwn0vAtRIKIRX6Y2pMl0RhIj8co9nBGFrvh55l3ngU7YObng7IVnFvGS+BYUpmHziY/Ls2zgP9SX50by/G9N5w6I+ogYvpwK1SoOlHQNsGfWcd9Peqof88B/rTyzF9hAIopAByQzC0JQB9ST5oVnvhnt+LOGsprvUhxNIwa0aY7cGR6Cp7tr8+whkjawIxkRWC6YJI6N+lAKq3Qf/Tx+B77oGfaQc/8hB8w2Xwtw9Bf3kzZspXY/JIDEbfpAB2BKLvVV90Jvjgoac9vpRxE8kciTVCBMMkNirJ7k/tRHyjtxwjKV4Yp3t/6s+R4E+/DH3N6+BrS8E314Dvvg2+/Sb4hxfBf5sP/up2TF3ZhonK1zD6dhwGdwail26DzqgX8MRKiq9ZBpkSkmeYOyPM3m9Jjl+1Z9D8AgNtlAq6bZ70qsZi+q+bwV/7I/hbB8D/dAr8Axq89iz474p/G5++koHJy1sx/lkGdBc2YjA3HF0rHNHuboomuQj/5DgclIvOGCGCYRKFFuTMV7YUAD3VDQaLMfyqBcZORGPy01QKYSNm/rYV/Nd/Av9NHvgbueBrsjDzRQamKKDxT9Kgq1iLkbIUDOSHoiNcgnYHgnYZi+9ZExSbiSoMc2eE2flKcuJLa4KGRQz6/U0wlGaP0feiMH4uFpMXEjBVlYjp6lWY+SSZtim0kulYMiYuJEJXuhTDJ9UYPByOvoIwdCxfgE4bAo0Jh39xLAoVpMwIEQyTyFCQvGpLon9sJ0K3J4OBDDcMH1dj9FQsxkrjMPFRPCbOx2GyfLal9VEcxstioTulxjAFNfROJPqLl6Bnfyg6V7ugz5yBhuHwrZjBdiU5YJg7I8wOpifAKoVIW7uQ3rpOBH2b3ekVjYT2WCRG3o+mIGKgO0OrlIaebU/HYOQDNbQnojB4NJyGD0NPfjA0bwTRE6Q7hsUcWhkWN8yZqSQlWWGECAZLmJfJmbrvVSI8taK37xpbdB/wQW8xPee/8xIGjvlj8IQ/hk4G0JbWcX8MHPVDX4kveoq8ocn3xLM33NCZRcPHOGJYZIKfpQyq7JjHS6yJjcHujLHADgkpuC7h8F8zEVqXSNC2awE69lqhs8AamkO26HrbDt2H7dBVQov2NcW26CiwQtu+BWjdY4n2nZboTbfCmKcCnRyDO/YmyLPnDlHvjDH8G6zhS9/wlEnYR7X00fWrFYuWdVI0ZpuhcbcczW/R2qdAcz6t/bRov4mONeaaoYl+p22rHF0bVNAmKtBvweIXGxNcfFH8eNlC4m6wMWMusEnKpn5hyo48pj9gLe4SNG9QoGGLAk8z5XiaJUd99u8122/IpBA2K9BGg2vWWKAvRYVeLzEa7E1R422m2+MsSTem97nSYnfKyN6/mzATv7AUgqcMrUnmaFlLX3ysM0fj+t/b5lQLtK22QEfyAmiSLKFZpUJ7kBRPXKW4HqCYynWVHKSG2LkyZex1uO1mZM9lKem9Tx9jjY5iNEYo0bKMhn7ZAu0r6H5PpLXCAq0rKJClSjSGynE/QIkrQYqBPe6S2X+AJsY2Ped6iWZk6RlL0c2r5szofRsO9R5S1IfQLRCpQL1aifoYFerpsbkuTImaUJXuXIDiH6/Ys8vm3Mg8L2i20YqsO7fItKLcSXyn0kXccclVqv3MS6at9JU/Ox+ouns+SF6Z4cSupz7l8+z1ucs7LF1AQjOdxfGZzmx8Iu1TRcfnrioICAQEAgIBgYBAQCAgEBAICAQEAgIBgYBAQCAgEBAICAQEAv8H44b/6ZiGvGAAAAAASUVORK5CYII=",
					"contentType": "image/png",
					"width": 15,
					"height": 15
				}
			}
		},
		"allowGeometryUpdates": true,
		"hasAttachments": true,
		"htmlPopupType": "esriServerHTMLPopupTypeNone",
		"hasM": false,
		"hasZ": false,
		"objectIdField": "OBJECTID",
		"uniqueIdField": {
			"name": "OBJECTID",
			"isSystemMaintained": true
		},
		"globalIdField": "",
		"typeIdField": "",
		"fields": [{
			"name": "OBJECTID",
			"type": "esriFieldTypeOID",
			"alias": "OBJECTID",
			"sqlType": "sqlTypeOther",
			"nullable": false,
			"editable": false,
			"domain": null,
			"defaultValue": null
		}],
		"indexes": [],
		"types": [],
		"templates": [{
			"name": "New Feature",
			"description": "",
			"drawingTool": "esriFeatureEditToolPoint",
			"prototype": {
				"attributes": {}
			}
		}],
		"supportedQueryFormats": "JSON, geoJSON",
		"hasStaticData": false,
		"maxRecordCount": 2000,
		"standardMaxRecordCount": 32000,
		"tileMaxRecordCount": 8000,
		"maxRecordCountFactor": 1,
		"capabilities": "Query,Editing,Create,Update,Delete,Sync",
		"syncEnabled": true,
		"adminLayerInfo": {
			"geometryField": {
				"name": "Shape",
				"srid": 102100
			}
		}
	}],
	"tables": []
}
```

##### Example of polyline layer

Property `geometryType` needs to be assigned to `esriGeometryPolyline`.

```js
{
	"layers": [{
		"currentVersion": 10.51,
		"id": 0,
		"name": "Line layer",
		"type": "Feature Layer",
		"displayField": "",
		"description": "",
		"copyrightText": "",
		"defaultVisibility": true,
		"editingInfo": {
			"lastEditDate": 1526947149024
		},
		"relationships": [],
		"isDataVersioned": false,
		"supportsAppend": true,
		"supportsCalculate": true,
		"supportsTruncate": true,
		"supportsAttachmentsByUploadId": true,
		"supportsAttachmentsResizing": true,
		"supportsRollbackOnFailureParameter": true,
		"supportsStatistics": true,
		"supportsAdvancedQueries": true,
		"supportsValidateSql": true,
		"supportsCoordinatesQuantization": true,
		"supportsApplyEditsWithGlobalIds": false,
		"supportsMultiScaleGeometry": true,
		"hasGeometryProperties": true,
		"geometryProperties": {
			"shapeLengthFieldName": "Shape__Length",
			"units": "esriMeters"
		},
		"advancedQueryCapabilities": {
			"supportsPagination": true,
			"supportsPaginationOnAggregatedQueries": true,
			"supportsQueryRelatedPagination": true,
			"supportsQueryWithDistance": true,
			"supportsReturningQueryExtent": true,
			"supportsStatistics": true,
			"supportsOrderBy": true,
			"supportsDistinct": true,
			"supportsQueryWithResultType": true,
			"supportsSqlExpression": true,
			"supportsAdvancedQueryRelated": true,
			"supportsCountDistinct": true,
			"supportsLod": true,
			"supportsReturningGeometryCentroid": false,
			"supportsReturningGeometryProperties": true,
			"supportsQueryWithDatumTransformation": true,
			"supportsHavingClause": true,
			"supportsOutFieldSQLExpression": true
		},
		"useStandardizedQueries": true,
		"geometryType": "esriGeometryPolyline",
		"minScale": 0,
		"maxScale": 0,
		"extent": {
			"xmin": -20037508.342788905,
			"ymin": -8175201.3721496435,
			"xmax": -10018754.171394452,
			"ymax": 12175461.54272524,
			"spatialReference": {
				"wkid": 102100
			}
		},
		"drawingInfo": {
			"renderer": {
				"type": "simple",
				"symbol": {
					"type": "esriSLS",
					"style": "esriSLSSolid",
					"color": [165, 83, 183, 255],
					"width": 1
				}
			},
			"transparency": 0,
			"labelingInfo": null
		},
		"allowGeometryUpdates": true,
		"hasAttachments": true,
		"htmlPopupType": "esriServerHTMLPopupTypeNone",
		"hasMetadata": true,
		"hasM": false,
		"hasZ": false,
		"objectIdField": "OBJECTID",
		"uniqueIdField": {
			"name": "OBJECTID",
			"isSystemMaintained": true
		},
		"globalIdField": "",
		"typeIdField": "",
		"fields": [{
			"name": "OBJECTID",
			"type": "esriFieldTypeOID",
			"alias": "OBJECTID",
			"sqlType": "sqlTypeOther",
			"nullable": false,
			"editable": false,
			"domain": null,
			"defaultValue": null
		}],
		"indexes": [],
		"types": [],
		"templates": [{
			"name": "New Feature",
			"description": "",
			"drawingTool": "esriFeatureEditToolLine",
			"prototype": {
				"attributes": {}
			}
		}],
		"supportedQueryFormats": "JSON, geoJSON",
		"hasStaticData": false,
		"maxRecordCount": 2000,
		"standardMaxRecordCount": 4000,
		"tileMaxRecordCount": 4000,
		"maxRecordCountFactor": 1,
		"capabilities": "Query,Editing,Create,Update,Delete,Sync",
		"syncEnabled": true,
		"adminLayerInfo": {
			"geometryField": {
				"name": "Shape",
				"srid": 102100
			}
		}
	}],
	"tables": []
}
```

##### Example of polygon layer

Property `geometryType` needs to be assigned to `esriGeometryPolygon`.

```js
{
	"layers": [{
		"currentVersion": 10.51,
		"id": 0,
		"name": "Polygon layer",
		"type": "Feature Layer",
		"displayField": "",
		"description": "",
		"copyrightText": "",
		"defaultVisibility": true,
		"editingInfo": {
			"lastEditDate": 1526947149828
		},
		"relationships": [],
		"isDataVersioned": false,
		"supportsAppend": true,
		"supportsCalculate": true,
		"supportsTruncate": true,
		"supportsAttachmentsByUploadId": true,
		"supportsAttachmentsResizing": true,
		"supportsRollbackOnFailureParameter": true,
		"supportsStatistics": true,
		"supportsAdvancedQueries": true,
		"supportsValidateSql": true,
		"supportsCoordinatesQuantization": true,
		"supportsApplyEditsWithGlobalIds": false,
		"supportsMultiScaleGeometry": true,
		"hasGeometryProperties": true,
		"geometryProperties": {
			"shapeAreaFieldName": "Shape__Area",
			"shapeLengthFieldName": "Shape__Length",
			"units": "esriMeters"
		},
		"advancedQueryCapabilities": {
			"supportsPagination": true,
			"supportsPaginationOnAggregatedQueries": true,
			"supportsQueryRelatedPagination": true,
			"supportsQueryWithDistance": true,
			"supportsReturningQueryExtent": true,
			"supportsStatistics": true,
			"supportsOrderBy": true,
			"supportsDistinct": true,
			"supportsQueryWithResultType": true,
			"supportsSqlExpression": true,
			"supportsAdvancedQueryRelated": true,
			"supportsCountDistinct": true,
			"supportsLod": true,
			"supportsReturningGeometryCentroid": true,
			"supportsReturningGeometryProperties": true,
			"supportsQueryWithDatumTransformation": true,
			"supportsHavingClause": true,
			"supportsOutFieldSQLExpression": true
		},
		"useStandardizedQueries": true,
		"geometryType": "esriGeometryPolygon",
		"minScale": 0,
		"maxScale": 0,
		"extent": {
			"xmin": -20037508.342788905,
			"ymin": -8175201.3721496435,
			"xmax": -9549097.23973764,
			"ymax": 12175461.54272524,
			"spatialReference": {
				"wkid": 102100
			}
		},
		"drawingInfo": {
			"renderer": {
				"type": "simple",
				"symbol": {
					"type": "esriSFS",
					"style": "esriSFSSolid",
					"color": [76, 129, 205, 191],
					"outline": {
						"type": "esriSLS",
						"style": "esriSLSSolid",
						"color": [0, 0, 0, 255],
						"width": 0.75
					}
				}
			},
			"transparency": 0,
			"labelingInfo": null
		},
		"allowGeometryUpdates": true,
		"hasAttachments": true,
		"htmlPopupType": "esriServerHTMLPopupTypeNone",
		"hasM": false,
		"hasZ": false,
		"objectIdField": "OBJECTID",
		"uniqueIdField": {
			"name": "OBJECTID",
			"isSystemMaintained": true
		},
		"globalIdField": "",
		"typeIdField": "",
		"fields": [{
			"name": "OBJECTID",
			"type": "esriFieldTypeOID",
			"alias": "OBJECTID",
			"sqlType": "sqlTypeOther",
			"nullable": false,
			"editable": false,
			"domain": null,
			"defaultValue": null
		}],
		"indexes": [],
		"types": [],
		"templates": [{
			"name": "New Feature",
			"description": "",
			"drawingTool": "esriFeatureEditToolPolygon",
			"prototype": {
				"attributes": {}
			}
		}],
		"supportedQueryFormats": "JSON, geoJSON",
		"hasStaticData": false,
		"maxRecordCount": 2000,
		"standardMaxRecordCount": 4000,
		"tileMaxRecordCount": 4000,
		"maxRecordCountFactor": 1,
		"capabilities": "Query,Editing,Create,Update,Delete,Sync",
		"syncEnabled": true,
		"adminLayerInfo": {
			"geometryField": {
				"name": "Shape",
				"srid": 102100
			}
		}
	}],
	"tables": []
}
```

##### Example of table

```js
{
	"layers": [{
		"currentVersion": 10.51,
		"id": 0,
		"name": "New_table",
		"type": "Table",
		"displayField": "",
		"description": "",
		"copyrightText": "",
		"defaultVisibility": true,
		"editingInfo": {
			"lastEditDate": null
		},
		"relationships": [],
		"isDataVersioned": false,
		"supportsAppend": true,
		"supportsCalculate": true,
		"supportsASyncCalculate": true,
		"supportsTruncate": true,
		"supportsAttachmentsByUploadId": true,
		"supportsAttachmentsResizing": true,
		"supportsRollbackOnFailureParameter": true,
		"supportsStatistics": true,
		"supportsExceedsLimitStatistics": true,
		"supportsAdvancedQueries": true,
		"supportsValidateSql": true,
		"supportsCoordinatesQuantization": true,
		"supportsFieldDescriptionProperty": true,
		"supportsQuantizationEditMode": true,
		"supportsApplyEditsWithGlobalIds": false,
		"advancedQueryCapabilities": {
			"supportsPagination": true,
			"supportsPaginationOnAggregatedQueries": true,
			"supportsQueryRelatedPagination": true,
			"supportsQueryWithDistance": true,
			"supportsReturningQueryExtent": true,
			"supportsStatistics": true,
			"supportsOrderBy": true,
			"supportsDistinct": true,
			"supportsQueryWithResultType": true,
			"supportsSqlExpression": true,
			"supportsAdvancedQueryRelated": true,
			"supportsCountDistinct": true,
			"supportsLod": true,
			"supportsQueryWithLodSR": false,
			"supportedLodTypes": ["geohash"],
			"supportsReturningGeometryCentroid": false,
			"supportsQueryWithDatumTransformation": true,
			"supportsHavingClause": true,
			"supportsOutFieldSQLExpression": true,
			"supportsMaxRecordCountFactor": true,
			"supportsTopFeaturesQuery": true,
			"supportsQueryWithCacheHint": true
		},
		"useStandardizedQueries": true,
		"allowGeometryUpdates": true,
		"hasAttachments": false,
		"htmlPopupType": "esriServerHTMLPopupTypeNone",
		"hasM": false,
		"hasZ": false,
		"objectIdField": "ObjectId",
		"uniqueIdField": {
			"name": "ObjectId",
			"isSystemMaintained": true
		},
		"globalIdField": "",
		"typeIdField": "",
		"fields": [{
			"name": "Start_Date",
			"type": "esriFieldTypeDate",
			"actualType": "datetime2",
			"alias": "Start Date",
			"sqlType": "sqlTypeTimestamp2",
			"length": 8,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Start_Time",
			"type": "esriFieldTypeDate",
			"actualType": "datetime2",
			"alias": "Start Time",
			"sqlType": "sqlTypeTimestamp2",
			"length": 8,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "End_Date",
			"type": "esriFieldTypeDate",
			"actualType": "datetime2",
			"alias": "End Date",
			"sqlType": "sqlTypeTimestamp2",
			"length": 8,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "End_Time",
			"type": "esriFieldTypeDate",
			"actualType": "datetime2",
			"alias": "End Time",
			"sqlType": "sqlTypeTimestamp2",
			"length": 8,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Event_Title",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Event Title",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "All_Day_Event",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "All Day Event",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "No_End_Time",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "No End Time",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Event_Description",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Event Description",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Contact",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Contact",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Contact_Email",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Contact Email",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Contact_Phone",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Contact Phone",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Location",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Location",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Category",
			"type": "esriFieldTypeInteger",
			"actualType": "int",
			"alias": "Category",
			"sqlType": "sqlTypeInteger",
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Mandatory",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Mandatory",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Registration",
			"type": "esriFieldTypeString",
			"actualType": "nvarchar",
			"alias": "Registration",
			"sqlType": "sqlTypeNVarchar",
			"length": 256,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Maximum",
			"type": "esriFieldTypeInteger",
			"actualType": "int",
			"alias": "Maximum",
			"sqlType": "sqlTypeInteger",
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "Last_Date_To_Register",
			"type": "esriFieldTypeDate",
			"actualType": "datetime2",
			"alias": "Last Date To Register",
			"sqlType": "sqlTypeTimestamp2",
			"length": 8,
			"nullable": true,
			"editable": true,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}, {
			"name": "ObjectId",
			"type": "esriFieldTypeOID",
			"actualType": "int",
			"alias": "ObjectId",
			"sqlType": "sqlTypeInteger",
			"nullable": false,
			"editable": false,
			"visible": true,
			"domain": null,
			"defaultValue": null
		}],
		"indexes": [{
			"name": "PK__STANDALO__9A6192912565F964",
			"fields": "ObjectId",
			"isAscending": true,
			"isUnique": true,
			"description": "clustered, unique, primary key"
		}],
		"types": [],
		"templates": [],
		"supportedQueryFormats": "JSON, geoJSON, PBF",
		"hasStaticData": true,
		"maxRecordCount": 2000,
		"standardMaxRecordCount": 32000,
		"tileMaxRecordCount": 8000,
		"maxRecordCountFactor": 1,
		"capabilities": "Query"
	}]
}
```

#### Change a table name

> **Endpoint name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/\<layerId\>/updateDefinition](https://developers.arcgis.com/rest/services-reference/update-definition-feature-layer-.htm)<br>
> **Example**: [https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update](
https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update)

The updateDefinition operation supports updating a definition property in a feature service layer. This will allow us for example to change name of the table/layer. The result of this operation is a response indicating success or failure with error code and description.

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/update-definition-feature-layer-.htm)

#### Add fields to a table

*PENDING*

#### Remove fields from a table

*PENDING*

#### Remove a table

> **Endpoint name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/deleteFromDefinition](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-service-.htm)<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition)

This operation supports deleting a definition property from a hosted feature service. For example we can remove a table from the database. The result of this operation is a response indicating success or failure with error code and description.

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-service-.htm)

### How to manage records in a table

*PENDING*

#### Query records

*PENDING*

Resources:

* [Query a feature layer](https://developers.arcgis.com/labs/rest/query-a-feature-layer/) (tutorial)
* [Using the ArcGIS REST Query Page](https://www.youtube.com/watch?v=LsYgtjkm69Y) (video)

#### Add records

> **Endpoint name**: [\<catalog-url\>/\<serviceName\>/FeatureServer/\<layerId\>/addFeatures](https://developers.arcgis.com/rest/services-reference/add-features.htm)<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/addFeatures](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/addFeatures)

This operation adds records to the associated feature layer or table.

If you are adding a record to a layer you will have to specify a geometry. Example:

```js
[{
	"attributes": {
		"field1": "value1",
		"field2": "value2"
	},
	"geometry": {
		"x": "-3.80704",
		"y": "43.46217"
	}
}, {
	"attributes": {
		"field1": "value1",
		"field2": "value2"
	},
	"geometry": {
		"x": "-3.80549",
		"y": "43.46242"
	}
}]
```

If you are adding a record to a table you will omit the `geometry` property. Example:

```js
[{
	"attributes": {
		"field1": "value1",
		"field2": "value2"
	}
}, {
	"attributes": {
		"field1": "value1",
		"field2": "value2"
	}
}]
```

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/add-features.htm).

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

* [ArcGIS REST API Youtube Playlist](https://www.youtube.com/watch?v=omPiCTXZ0U8&list=PLahIW2YFPQd7o6L9fxSAXCuv5FldGkycP)

Other written resources:

* [Esri Open Vision > Open Specs > ArcGIS REST API](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/arcgis-rest-api/)
