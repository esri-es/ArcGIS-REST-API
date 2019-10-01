# Hosted Feature Service - Services reference

We are aware that the documentation from the [ArcGIS REST API](https://developers.arcgis.com/rest/services-reference/get-started-with-the-services-directory.htm) can be overwhelming to new ArcGIS Developers, that why  we created this focused resource.

This resource has been created for them, so it we will focus on explaining how to host data in [ArcGIS Online](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-online/) (the Esri cloud product), although they work in an equivalent way in [ArcGIS Enterprise](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-enterprise/) (the Esri on-premises product).

As far as possible we will try to avoid the technicalities of geographic information systems in order to facilitate understanding.

The [Postman collection included](./Hosted%20Feature%20Service%20-%20ArcGIS.postman_collection.json) is aimed at facilitating the use and understanding of [the official documentation](https://developers.arcgis.com/rest/services-reference/hosted-feature-service.htm) serving as a complement to it.

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of contents

- [Getting started](#getting-started)
- [Previous concepts](#previous-concepts)
  - [ArcGIS Online items (ArcGIS geoinformation model)](#arcgis-online-items-arcgis-geoinformation-model)
  - [Applications using the REST API & Reverse engineering](#applications-using-the-rest-api--reverse-engineering)
  - [ArcGIS Online Architecture](#arcgis-online-architecture)
- [Cheatsheet](#cheatsheet)
  - [Quick reference](#quick-reference)
- [Working with Databases (Feature Services)](#working-with-databases-feature-services)
  - [How to check if a database exists](#how-to-check-if-a-database-exists)
  - [How to create an empty database](#how-to-create-an-empty-database)
  - [How to get database info](#how-to-get-database-info)
  - [How to change database info](#how-to-change-database-info)
  - [How to manage database options](#how-to-manage-database-options)
    - [Visibility](#visibility)
    - [Edition, manage indexes, cache control and allow export data](#edition-manage-indexes-cache-control-and-allow-export-data)
  - [How to remove a database](#how-to-remove-a-database)
- [Working with tables (Feature Layers and Tables)](#working-with-tables-feature-layers-and-tables)
  - [How to manage tables](#how-to-manage-tables)
    - [Create a table](#create-a-table)
    - [Change table properties](#change-table-properties)
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
- [FAQ](#faq)
  - [Where do I have to sign up?](#where-do-i-have-to-sign-up)
  - [Do I have to pay to host my data in ArcGIS Online?](#do-i-have-to-pay-to-host-my-data-in-arcgis-online)
  - [Is there any SDK available to facilitate the development?](#is-there-any-sdk-available-to-facilitate-the-development)
- [Additional resources](#additional-resources)
  - [Resources is Spanish](#resources-is-spanish)

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

And this is a very basic example of the JSON that describes how a **Hosted Feature Service item**  looks like:

> **Note**: *some properties might differ based on the item type.*

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
* [Web GIS and geoinformation model](https://www.youtube.com/watch?v=MSJetLXD-Cw&list=PLoptan2utx16J4O_JpRqYiTxpATG1QKjD&index=2) (video in Spanish)
* [ArcGIS Platform basic concepts](https://youtu.be/5WLmzvtwJqg?t=532) (video in Spanish)

### Applications using the REST API & Reverse engineering

The API is used by all [Esri products](https://docs.google.com/drawings/d/1w_tBCVPdPULUehfFBJaqyH7ywZyxZLBCeemWFa3o2Ho/edit?usp=sharing), [APIs and SDKs](https://developers.arcgis.com/documentation/#sdks). By this we mean that **everything that can be done through a client can also be done through the API**.

For this reason we believe that the most useful thing in order to get the most out of the API is to know how to reverse engineer these interfaces/SDKs in order to replicate and debug any functionality that is implemented in these clients (as is shown in [this talk](https://youtu.be/uJvZ8MJA0t4?t=208)).

> You can reverse engineer this interfaces using several free applications like: [Fiddler](https://www.telerik.com/fiddler), [Postman Interceptor](https://learning.getpostman.com/docs/postman/sending_api_requests/interceptor_extension/), the [Network tab at Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/network), etc

### ArcGIS Online Architecture

ArcGIS online is **much more** than hosted feature services, that's we we encourage you to take a look to this [ArcGIS Online resource page](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-online/) and this simplified diagram of the ArcGIS Online Architecture to have better understanding on how it works:

[![ArcGIS Online Architecture](https://esri-es.github.io/awesome-arcgis/arcgis/products/product-thumbnails/arcgis-online.png)](https://docs.google.com/drawings/d/e/2PACX-1vRbMbrSXgE8fGdsIz5RBgGNpixoPPgJ6swlk9vT3lUyW8cOffUxmb3Oludm7yF44BzwRoTPtZ5jvwGx/pub?w=3870&amp;h=2405)

## Cheatsheet

|URL shortcut|Example|Description
|---|---|---
|`<root-url>`|`https://www.arcgis.com/sharing/rest/`|ArcGIS Online API root
|`<admin-catalog-url>`|`https://<servicesX>.arcgis.com/<instance>/arcgis/rest/admin/services/`|Root folder to admin a hosted feature service
|`<catalog-url>`|`https://<servicesX>.arcgis.com/<instance>/arcgis/rest/services/`|Root folder to manage a hosted feature service

> **Note**: an `<instance>` is equivalent to an `<organizationId>` and looks like this: `rF1wdZICHfgsvter`

### Quick reference

|Endpoint|Method|Tasks|
|---|---|---|
|`<root-url>/portals/<instance>/isServiceNameAvailable`|GET|[Check if a database exists](#how-to-check-if-a-database-exists)
|`<root-url>/content/users/<username>/createService`|POST|[Create an empty database](#how-to-create-an-empty-database)
|`<root-url>/content/users/<username>/shareItems`|POST|[Change database visibility](#visibility)
|`<root-url>/content/users/<username>/items/<itemId>/update`|POST|[Change database info (title, description, tags, ...)](#how-to-change-database-info)
|`<root-url>/content/users/<username>/items/<itemId>/delete`|POST|[Remove a database](#how-to-remove-a-database)
|`<admin-catalog-url>/<serviceName>/FeatureServer/addToDefinition`|POST|[Add a table or layer to a database](#create-a-table)
|`<admin-catalog-url>/<serviceName>/FeatureServer/updateDefinition`|POST|[Change database permissions, indexes, cache, ...](#edition-manage-indexes-cache-control-and-allow-export-data)
|`<admin-catalog-url>/<serviceName>/FeatureServer/deleteFromDefinition`|POST|[Remove a table or layer from a database](#remove-a-table)
|`<admin-catalog-url>/<serviceName>/FeatureServer/<layerId>/addToDefinition`|POST|[Add fields to a table or layer](#add-fields-to-a-table)
|`<admin-catalog-url>/<serviceName>/FeatureServer/<layerId>/updateDefinition`|POST|[Change a table or layer name, update drawing info, allow attachments, time settings, ...](#change-table-properties)
|`<catalog-url>/<serviceName>/FeatureServer`|GET|[Get database info](#how-to-get-database-info)
|`<catalog-url>/<serviceName>/FeatureServer/<layerId>/addFeatures`|POST|[Add records to a table or layer](#add-records)



## Working with Databases (Feature Services)

### How to check if a database exists

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<root-url\>/portals/\<instance\>/isServiceNameAvailable](https://developers.arcgis.com/rest/users-groups-and-items/check-service-name.htm)<br>
> **Example**:
[https://www.arcgis.com/sharing/rest/portals/rF1wdZICHfgsvter/isServiceNameAvailable?name=Testing_purposes_POSTMAN_Collection&f=json&token=...&type=Feature%20Service](https://www.arcgis.com/sharing/rest/portals/rF1wdZICHfgsvter/isServiceNameAvailable?name=Testing_purposes_POSTMAN_Collection&f=json&token=...&type=Feature20Service)

Checks whether a given service name and type are available for publishing a new service. true indicates that the name and type is not found in the organization's services and is available for publishing. false means the requested name and type are not available.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/users-groups-and-items/check-service-name.htm)
* Sample GUIs using this endpoint:
	* [First step in the New Layer creation process @ developers.arcgis.com](https://developers.arcgis.com/layers/new)
	* [Create feature Layer from My Content @ arcgis.com/home/content.html](https://www.esri.com/arcgis-blog/products/arcgis-online/announcements/creating-an-empty-feature-layer-for-data-collection/)

### How to create an empty database

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<root-url\>/content/users/\<username\>/createService](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm) <br>
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
	* `https://www.arcgis.com/sharing/rest/content/users/<username>/<operation>`
	* `https://www.arcgis.com/sharing/rest/content/users/<username>/items/<itemId>/<operation>`
* To manage the Hosted Feature Service we will use something like:
	* `https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/<serviceName>/FeatureServer/<operation>`
	* `https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/<serviceName>/FeatureServer/<layerId>/<operation>`

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm)
* Sample GUIs using this endpoint:
	* [Add > New Layer @ developers.arcgis.com](https://developers.arcgis.com/layers/new)
	* [Create feature Layer from My Content @ arcgis.com/home/content.html](https://www.esri.com/arcgis-blog/products/arcgis-online/announcements/creating-an-empty-feature-layer-for-data-collection/)

### How to get database info

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<catalog-url\>/\<serviceName\>/FeatureServer](https://developers.arcgis.com/rest/services-reference/feature-service.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json](
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json)

This resource provides basic information about the feature service, including the feature layers and tables that it contains, the service description, and so on.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/feature-service.htm)
* Sample GUIs using this endpoint:
	* [Item details page > Overview](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm) ([sample page](https://geogeeks.maps.arcgis.com/home/item.html?id=99fd67933e754a1181cc755146be21ca))

### How to change database info

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<root-url\>/content/users/\<username\>/items/\<itemId\>/update](https://developers.arcgis.com/rest/services-reference/feature-service.htm)<br>
> **Example**: [
https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update](
https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update)

Update item information and their file, URL, or text depending on type. Users can use this operation to update item information such as title, description, tags, and so on, or use it to update an item's file, URL, or text. This call is available to the user and the administrator of the organization.

The parameters that are to be updated need to be specified in the request. Parameters not specified will not be affected.

All parameters for this operation are optional.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/users-groups-and-items/update-item.htm)
* Sample GUIs using this endpoint:
	* [Item details page > Edit](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)

### How to manage database options

#### Visibility

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<root-url\>/content/users/\<username\>/shareItems](https://developers.arcgis.com/rest/users-groups-and-items/share-items.htm)<br>
> **Example**: [https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/shareItems](https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/shareItems)

This operation allows to share a database (including all contained tables) with:

* Everyone, in which case they are publicly accessible
* With any user in an ArcGIS organization
* With a list of user groups within ArcGIS.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/users-groups-and-items/share-items.htm).
* Sample GUIs using this endpoint:
    * ["My content" section](https://doc.arcgis.com/en/arcgis-online/share-maps/share-items.htm) ([arcgis.com/home/content.html](https://geogeeks.maps.arcgis.com/home/content.html))
    * [Item details page > Share](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)

#### Edition, manage indexes, cache control and allow export data

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Endpoint**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/updateDefinition](https://developers.arcgis.com/rest/services-reference/update-definition-feature-service-.htm)
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/updateDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/updateDefinition)

This operation allows us to change who is able to edit the service, who can do what, if we want to keep track of who created and updated records and when, manage spatial indexes, enable cache (CDN), and manage if export data is publicly for everyone. The result of this operation is a response indicating success or failure with error code and description.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/update-definition-feature-service-.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Settings](https://doc.arcgis.com/en/arcgis-online/manage-data/manage-hosted-feature-layers.htm)

### How to remove a database

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Endpoint**: [\<root-url\>/content/users/\<username\>/items/\<itemId\>/delete](https://developers.arcgis.com/rest/users-groups-and-items/delete-item.htm)
> **Example**: [https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/ae4a188ab58c4cf9b5247d20cdb40206/delete](https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/ae4a188ab58c4cf9b5247d20cdb40206/delete)

This operation removes both the item and its link from the user's folder by default. For example, you can use it to remove a database.

This operation is available to the user and to the administrator of the organization to which the user belongs.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/users-groups-and-items/delete-item.htm)
https://doc.arcgis.com/en/arcgis-online/administer/manage-items.htm
* Sample GUIs using this endpoint:
    * ["My content > Check > Share" section and the "Item details page > Settings > Delete item"](https://doc.arcgis.com/en/arcgis-online/administer/manage-items.htm)

## Working with tables (Feature Layers and Tables)

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

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

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/addToDefinition](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-service-.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition)

This operation allows us to add a table to a database (`<serviceName>` param). The result of this operation is a response indicating success or failure with error code and description.

In the `addToDefinition` param you have to define which layer type you want to create.

You can learn more about the [serviceDefiniton here](./serviceDefinition.md).

* [Example of point layer](./serviceDefinition.md#point-layer)
* [Example of polyline layer](./serviceDefinition.md#polyline-layer)
* [Example of polygon layer](./serviceDefinition.md#polygon-layer)
* [Example of standalone table](./serviceDefinition.md#standalone-table)

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-service-.htm).
* Sample GUIs using this endpoint:
    * [Add > New Layer @ developers.arcgis.com](https://developers.arcgis.com/layers/new)
    * [Create feature Layer from My Content @ arcgis.com/home/content.html](https://www.esri.com/arcgis-blog/products/arcgis-online/announcements/creating-an-empty-feature-layer-for-data-collection/)

#### Change table properties

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/\<layerId\>/updateDefinition](https://developers.arcgis.com/rest/services-reference/update-definition-feature-layer-.htm)<br>
> **Example**: [https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update](
https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/08fe7baa579e4661a2291f25c4c1e05b/update)

The updateDefinition operation supports updating a definition property in a feature service layer. This will allow us for example to:

* Change name of the table/layer
* Update the drawing info when adding a new field to the layer, etc.
* Enable/disable [attachments](https://enterprise.arcgis.com/en/portal/latest/use/manage-hosted-feature-layers.htm#ESRI_SECTION1_3FA256DBA3D94BBB9F21CA1481E656E3)
* Change [Time Settings](https://doc.arcgis.com/en/arcgis-online/create-maps/configure-time.htm)

The result of this operation is a response indicating success or failure with error code and description.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/update-definition-feature-layer-.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Layers > Edit](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)

#### Add fields to a table

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/\<layerId\>/addToDefinition](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-layer-.htm)<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/0/addToDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/0/addToDefinition)

This operation supports adding a definition property in a hosted feature service layer, for example add a new field. The result of this operation is a response indicating success or a response indicating failure with an error code and description.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-layer-.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Data > Fields](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)
    * [Web map editor](https://www.esri.com/arcgis-blog/products/arcgis-online/sharing-collaboration/how-to-create-a-hosted-feature-service/)

#### Remove fields from a table

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

* Sample GUIs using this endpoint:
    * [Item details page > Data > Fields](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)

#### Remove a table

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<admin-catalog-url\>/\<serviceName\>/FeatureServer/deleteFromDefinition](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-service-.htm)<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition)

This operation supports deleting a definition property from a hosted feature service. For example we can remove a table from the database. The result of this operation is a response indicating success or failure with error code and description.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-service-.htm)

### How to manage records in a table

#### Query records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Query a feature layer](https://developers.arcgis.com/labs/rest/query-a-feature-layer/) (tutorial)
* [Using the ArcGIS REST Query Page](https://www.youtube.com/watch?v=LsYgtjkm69Y) (video)
* Sample GUIs using this endpoint:


#### Add records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: [\<catalog-url\>/\<serviceName\>/FeatureServer/\<layerId\>/addFeatures](https://developers.arcgis.com/rest/services-reference/add-features.htm)<br>
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

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/add-features.htm).
* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)
* Sample GUIs using this endpoint:

#### Update records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)
* Sample GUIs using this endpoint:

#### Delete records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)
* Sample GUIs using this endpoint:

#### Add attachments to a record

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

### How to manager layer views (filter / extend tables)

#### Create a layer view

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* Sample GUIs using this endpoint:

#### Delete a layer view

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* Sample GUIs using this endpoint:

### How to manage a relationship

#### Create a relationship

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* Sample GUIs using this endpoint:

#### Query a relationship

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* Sample GUIs using this endpoint:

#### Delete a relationship

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

*PENDING*

**Resources**:

* [Full documentation](#).
* Sample GUIs using this endpoint:

## FAQ

### Where do I have to sign up?

There are three types of accounts in ArcGIS Online, so be careful and **do not** create a [public account](https://www.arcgis.com/sharing/rest/oauth2/signup?client_id=arcgisonline&redirect_uri=http://www.arcgis.com&response_type=token) or an [21-day trial organizational account](https://www.esri.com/en-us/arcgis/products/arcgis-online/trial), **create a [free developer account](http://developers.arcgis.com/sign-up)**.

### Do I have to pay to host my data in ArcGIS Online?

*PENDING*

> How credits related to hosted feature services works

### Is there any SDK available to facilitate the development?

Yes, there are two, so if you are a JavaScript or a Python developer you might want to take a look to the available SDKs:

* [ArcGIS API for Python](https://github.com/Esri/arcgis-python-api)
* [ArcGIS REST JS](https://esri.github.io/arcgis-rest-js/api/)

**Note**: *none of them have support to every capability of the REST API, that's why we still think it's worth learning how to use it directly.*ss

## Additional resources

Videos:

* [ArcGIS REST API Youtube Playlist](https://www.youtube.com/watch?v=omPiCTXZ0U8&list=PLahIW2YFPQd7o6L9fxSAXCuv5FldGkycP)

Other written resources:

* [Esri Open Vision > Open Specs > ArcGIS REST API](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/arcgis-rest-api/)


### Resources is Spanish

* [Introduction to webmaps: What they are, when to use them, pros and limitations](https://hhkaos.github.io/esri-mooc/plataforma-arcgis/desarrolladores-web/web-maps/introduccion)
* [Webmap specification: JSON format details](https://hhkaos.github.io/esri-mooc/plataforma-arcgis/desarrolladores-web/web-maps/especificacion)
* [When to use web maps/scenes](https://hhkaos.github.io/esri-mooc/plataforma-arcgis/desarrolladores-web/web-maps/cuando-utilizarlos)
* [Manipulate web maps](https://hhkaos.github.io/esri-mooc/plataforma-arcgis/desarrolladores-web/web-maps/manipular-web-maps)
* [Consume web maps/scenes using JavaScript](https://hhkaos.github.io/esri-mooc/plataforma-arcgis/desarrolladores-web/web-maps/consumir-usando-js)
* [Web maps: Tips and tricks](https://hhkaos.github.io/esri-mooc/plataforma-arcgis/desarrolladores-web/web-maps/consejos-y-trucos)
