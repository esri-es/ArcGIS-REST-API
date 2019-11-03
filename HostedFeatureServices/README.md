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
  - [ArcGIS Online Architecture and Deployment](#arcgis-online-architecture-and-deployment)
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
    - [Get table info](#get-table-info)
    - [Change table properties](#change-table-properties)
    - [Add fields to a table](#add-fields-to-a-table)
    - [Remove fields from a table](#remove-fields-from-a-table)
    - [Remove a table](#remove-a-table)
  - [How to manage records in a table](#how-to-manage-records-in-a-table)
    - [Add records](#add-records)
    - [Query records](#query-records)
    - [Add, update and delete multiple records with a single call](#add-update-and-delete-multiple-records-with-a-single-call)
    - [Delete records](#delete-records)
    - [Add attachments to a record](#add-attachments-to-a-record)
  - [How to manage layer views (filter / extend tables)](#how-to-manage-layer-views-filter--extend-tables)
    - [Create a layer view](#create-a-layer-view)
      - [Content from individual layers](#content-from-individual-layers)
      - [Content from joined layers](#content-from-joined-layers)
    - [Filter information in a layer view](#filter-information-in-a-layer-view)
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

You are ready to go, feel free to explore. This documentation is written by describing [common use cases](https://github.com/esri-es/ArcGIS-REST-API/tree/master/HostedFeatureServices#table-of-contents), but we highly encourage you to read the "Previous concepts" first. But in case you are already familiar with the ArcGIS platform and just want to know how to create an empty database using the REST API:

1. Go to **"[How to create an empty database](#how-to-create-an-empty-database)"** section.
2. Read the documentation
3. Find in Postman the ***Endpoint name*** and test it

> If you have any other questions please use the [repository issues](https://github.com/esri-es/ArcGIS-REST-API/issues).

## Previous concepts

When working with hosted feature services is better to be familiar with the ArcGIS [geoinformation model](https://doc.arcgis.com/en/arcgis-online/reference/geo-info.htm) and the [ArcGIS Online Architecture](#arcgis-online-architecture-and-deployment) in order to better understand why the endpoints are designed the way they are.

### ArcGIS Online items (ArcGIS geoinformation model)

> **Note**: items ArcGIS Enterprise works the same way

An `Item` is the core unit of the geoinformation model. To better understand the concept of an `Item`, it is necessary to bear in mind that ArcGIS Online is designed from its base to be used for organizations with multiple users (not just developers), and so that any user of an organization (especially those without programming skills) can create, share, find and manage with other users all kinds of geolocalized information:

* **Data through**: databases and static files (CSV, GeoJSON, Excels, ...)
* **Static files**: PDFs, ZIPs, etc
* **Astonishing 2D and 3D maps**: possible thanks to the ArcGIS map specifications like [Web Maps](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/web-map/) and [Web Scenes](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/web-map/)
* **Configuration files for apps**
* And much more

Bearing this in mind, when we create a free developer account we are assigned an organizational account that can only have a single user, so there are some capabilities such as sharing an item with a group that will not be very useful, as there are no more users in our organization.

> The only exception is that we want to group several public items in a public group for reasons of organization and visibility of the items.

So, to keep it simple: an `Item` is just a JSON plain object stored in ArcGIS which contains information about a resource (database, static file, etc) like permissions and other properties.

As a first approach, imagine each user in ArcGIS Online has an item table associated with it, something like this:

[![Item table](https://user-images.githubusercontent.com/826965/65668230-a0fde280-e041-11e9-8d8b-8347483291be.png)](https://user-images.githubusercontent.com/826965/65668230-a0fde280-e041-11e9-8d8b-8347483291be.png)

> **Important** Note that the number and role of the properties of each element may vary depending on the `type` of element, for example, in the diagram above the property: URL.

This is a very basic example of the JSON that describes how a **Hosted Feature Service item**  looks like:

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

Knowing this is enough for now, although we leave you some additional resources in case you're curious:

* [Supported item types](https://doc.arcgis.com/en/arcgis-online/reference/supported-items.htm)
* [Items and item types](https://developers.arcgis.com/rest/users-groups-and-items/items-and-item-types.htm) (developer documentation)
* [Web GIS and geoinformation model](https://www.youtube.com/watch?v=MSJetLXD-Cw&list=PLoptan2utx16J4O_JpRqYiTxpATG1QKjD&index=2) (video in Spanish)
* [ArcGIS Platform basic concepts](https://youtu.be/5WLmzvtwJqg?t=532) (video in Spanish)

### Applications using the REST API & Reverse engineering

The API is used by all [Esri products](https://docs.google.com/drawings/d/1w_tBCVPdPULUehfFBJaqyH7ywZyxZLBCeemWFa3o2Ho/edit?usp=sharing), [APIs and SDKs](https://developers.arcgis.com/documentation/#sdks), by this we mean that **everything that can be done through a client can also be done through the API**.

For this reason we believe that the most useful thing in order to get the most out of the API is to know how to reverse engineer these interfaces/SDKs in order to replicate and debug any functionality that is implemented in these clients (as is shown in [this talk](https://youtu.be/uJvZ8MJA0t4?t=208)).

> You can reverse engineer this interfaces using several free applications like: [Fiddler](https://www.telerik.com/fiddler), [Postman Interceptor](https://learning.getpostman.com/docs/postman/sending_api_requests/interceptor_extension/), the [Network tab at Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/network), etc

### ArcGIS Online Architecture and Deployment

ArcGIS online is **much more** than hosted feature services, that's we we encourage you to take a look to this [ArcGIS Online resource page](https://esri-es.github.io/awesome-arcgis/arcgis/products/arcgis-online/) and this simplified diagram of the ArcGIS Online Architecture to have better understanding on how it works:

[![ArcGIS Online Architecture](https://docs.google.com/drawings/d/e/2PACX-1vRbMbrSXgE8fGdsIz5RBgGNpixoPPgJ6swlk9vT3lUyW8cOffUxmb3Oludm7yF44BzwRoTPtZ5jvwGx/pub?w=3870&h=2405)](https://docs.google.com/drawings/d/e/2PACX-1vRbMbrSXgE8fGdsIz5RBgGNpixoPPgJ6swlk9vT3lUyW8cOffUxmb3Oludm7yF44BzwRoTPtZ5jvwGx/pub?w=3870&amp;h=2405)

If you want an **overview about the Esri product offering to work with ArcGIS Online deployment** this is another nice diagram:

[![ArcGIS Online deployment](https://docs.google.com/drawings/d/e/2PACX-1vSqd_TTJABumrhzMKS64-G9SIRxhJ-vVAufeTKI-c8Nrwc0clDQBpU83_Sv6-GbuPwbrdrwsVHqynPY/pub?w=1650&amp;h=928)](https://docs.google.com/drawings/d/1fTgq1he-RmjQwqcV_lBfeMRAo8w5TzmMomfMxfRxUiY/edit?usp=sharing)

## Cheatsheet

|URL shortcut|Example|Description
|---|---|---
|`<root-url>`|`https://www.arcgis.com/sharing/rest/`|ArcGIS Online API root
|`<admin-catalog-url>`|`https://<servicesX>.arcgis.com/<instance>/arcgis/rest/admin/services/`|Root folder to admin a hosted feature service
|`<catalog-url>`|`https://<servicesX>.arcgis.com/<instance>/arcgis/rest/services/`|Root folder to manage a hosted feature service

> **Note**: an `<instance>` is equivalent to an `<organizationId>` and looks like this: `rF1wdZICHfgsvter`. And the subdomain `<servicesX>` will be returned by the API when you create a new service, and as you'll see, all your services will share the same subdomain, and looks like this: `services6`, `services7`, `services8`, ...

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

> **Postman request name**: `<root-url>/portals/<instance>/isServiceNameAvailable`<br>
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

> **Postman request name**: `<root-url>/content/users/<username>/createService` <br>
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

> **Postman request name**: `<catalog-url>/<serviceName>/FeatureServer`<br>
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

> **Postman request name**: `<root-url>/content/users/<username>/items/<itemId>/update`<br>
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

> **Postman request name**: `<root-url>/content/users/<username>/shareItems`<br>
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

> **Endpoint**: `<admin-catalog-url>/<serviceName>/FeatureServer/updateDefinition`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/updateDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/updateDefinition)

This operation allows us to change who is able to edit the service, who can do what, if we want to keep track of who created and updated records and when, manage spatial indexes, enable cache (CDN), and manage if export data is publicly for everyone. The result of this operation is a response indicating success or failure with error code and description.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/update-definition-feature-service-.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Settings](https://doc.arcgis.com/en/arcgis-online/manage-data/manage-hosted-feature-layers.htm)

### How to remove a database

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Endpoint**: `<root-url>/content/users/<username>/items/<itemId>/delete`<br>
> **Example**: [https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/ae4a188ab58c4cf9b5247d20cdb40206/delete](https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/items/ae4a188ab58c4cf9b5247d20cdb40206/delete)

This operation removes both the item and its link from the user's folder by default. For example, you can use it to remove a database.

This operation is available to the user and to the administrator of the organization to which the user belongs.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/users-groups-and-items/delete-item.htm)
https://doc.arcgis.com/en/arcgis-online/administer/manage-items.htm
* Sample GUIs using this endpoint:
    * ["My content > Check > Share" section and the "Item details page > Settings > Delete item"](https://doc.arcgis.com/en/arcgis-online/administer/manage-items.htm)

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

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<admin-catalog-url>/<serviceName>/FeatureServer/addToDefinition`<br>
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
    * [ArcGIS REST Admin Service Page > Add To Definition](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/admin/services/NewFeatureService/FeatureServer/addToDefinition)


#### Get table info

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<admin-catalog-url>/<serviceName>/FeatureServer/<layerId>`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer/0?&f=pjson](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer/0?&f=pjson)

The layer resource represents a single feature layer or a non-spatial table in a feature service. A feature layer is a table or view with at least one spatial column.

For tables, it provides basic information about the table such as its ID, name, fields, types and templates.

For feature layers, in addition to the table information above, it provides information such as its geometry type, min and max scales, and spatial reference.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/feature-layer.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Data](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1#data) ([Documentation]((https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)))
    * [ArcGIS REST Admin Service Page > Layer](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/NewFeatureService/FeatureServer/0)


#### Change table properties

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<admin-catalog-url>/<serviceName>/FeatureServer/<layerId>/updateDefinition`<br>
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
    * [ArcGIS REST Admin Service Page > Layer > Update Definition](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/admin/services/NewFeatureService/FeatureServer/0/updateDefinition)

#### Add fields to a table

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<admin-catalog-url>/<serviceName>/FeatureServer/<layerId>/addToDefinition`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/0/addToDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/0/addToDefinition)

This operation supports adding a definition property in a hosted feature service layer, for example add a new field. The result of this operation is a response indicating success or a response indicating failure with an error code and description.

When adding a new field the API expects in the `addToDefinition` something like this:

```js
{
	"fields": [{
		"name": "NewField1",
		"type": "esriFieldTypeString",
		"alias": "New Field1",
		"nullable": true,
		"editable": true,
		"length": 256
	}, {
		"name": "NewField2",
		"type": "esriFieldTypeDouble",
		"alias": "New Field2",
		"nullable": true,
		"editable": true
	}]
}
```

More about the [field spec](https://github.com/esri-es/ArcGIS-REST-API/blob/master/HostedFeatureServices/serviceDefinition.md#field-model-object)

> **Supported types**: esriFieldTypeBlob | esriFieldTypeDate | esriFieldTypeDouble | esriFieldTypeGeometry | esriFieldTypeGlobalID | esriFieldTypeGUID | esriFieldTypeInteger | esriFieldTypeOID | esriFieldTypeRaster | esriFieldTypeSingle | esriFieldTypeSmallInteger | esriFieldTypeString | esriFieldTypeXML. More at: [Supported data types](https://developers.arcgis.com/documentation/common-data-types/field.htm) | [correspondence between **type** and **database column type**](https://developers.arcgis.com/rest/services-reference/layer-feature-service-.htm#UL_01B2204C60864812A6E013ACD589E881)

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-layer-.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Data > Fields](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)
    * [ArcGIS REST Admin Service Page > Layer > Add To Definition](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/admin/services/NewFeatureService/FeatureServer/0/addToDefinition)

#### Remove fields from a table

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<admin-catalog-url>/<serviceName>/FeatureServer/<layerId>/deleteFromDefinition`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/0/deleteFromDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/0/deleteFromDefinition)

This operation supports deleting a definition property in a hosted feature service layer, for example remove a field. The result of this operation is a response indicating success or a response indicating failure with an error code and description. [Learn more](https://github.com/esri-es/ArcGIS-REST-API/tree/master/HostedFeatureServices#remove-fields-from-a-table).

* [Full documentation](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-layer-.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Data > Fields](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)
    * [ArcGIS REST Admin Service Page > Layer > Delete From Definition](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/admin/services/NewFeatureService/FeatureServer/0/deleteFromDefinition)

#### Remove a table

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<admin-catalog-url>/<serviceName>/FeatureServer/deleteFromDefinition`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition)

This operation supports deleting a definition property from a hosted feature service. For example we can remove a table from the database. The result of this operation is a response indicating success or failure with error code and description.

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/delete-from-definition-feature-service-.htm)
* Sample GUIs using this endpoint:
    * [Organization content page](https://www.arcgis.com/home/content.html): Check the item a click "Delete"
    * [Item details page > Settings > Delete item](https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)
    * [ArcGIS REST Admin Service Page > Delete From Definition](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/admin/services/NewFeatureService/FeatureServer/deleteFromDefinition)

### How to manage records in a table

#### Add records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<catalog-url>/<serviceName>/FeatureServer/<layerId>/addFeatures`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/addFeatures](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/addFeatures)

This operation adds records to the associated feature layer or table.

If you are adding a record to a layer you will have to specify a geometry.

Example:

```js
[{
	"attributes": {
		"NewField1": "value1",
		"NewField2": 1.1
	},
	"geometry": {
		"x": "-3.80704",
		"y": "43.46217"
	}
}, {
	"attributes": {
		"NewField1": "value3",
		"NewField2": 1.2
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
		"NewField1": "value1",
		"NewField2": 1.1
	}
}, {
	"attributes": {
		"NewField1": "value3",
		"NewField2": 1.2
	}
}]
```

**Resources:**

* [Full documentation](https://developers.arcgis.com/rest/services-reference/add-features.htm).
* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)
* Sample GUIs using this endpoint:
    * [Web map viewer > Edit > Add features](https://www.arcgis.com/home/webmap/viewer.html?url=https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer&source=sd) (just for layers, not for tables)
    * [ArcGIS REST Service Page > Layer/Table > Add features](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/addFeatures)

#### Query records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<catalog-url>/<serviceName>/FeatureServer/<layerId>/query`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/query?f=pjson&where=1=1&outFields=*](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/query?f=pjson&where=1=1&outFields=*)

The result of this operation is either a feature set or an array of feature IDs (if `returnIdsOnly` is set to `truev) and/or a result extent (if `returnExtentOnly` is set to `true`).

While there is a limit to the number of features included in the feature set response, there is no limit to the number of object IDs returned in the ID array response. Clients can exploit this to get all the query conforming object IDs by specifying returnIdsOnly=true and subsequently requesting feature sets for subsets of object IDs.

> **Note**: A table/layer use to paginate results (default limit is 2000 records per query), but you can [check the property `maxRecordCount` from your table metadata](#get-table-info). In case you need to recover more pages you can use the parameter `resultOffset`.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/query-feature-service-layer-.htm).
* [Query a feature layer](https://developers.arcgis.com/labs/rest/query-a-feature-layer/) (tutorial)
* [Using the ArcGIS REST Query Page](https://www.youtube.com/watch?v=LsYgtjkm69Y) (video)
* Sample GUIs using this endpoint:
    * [Item details page > Data](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1#data) ([Documentation]((https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)))
    * [Item details page > Visualization](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1#visualize) ([Documentation]((https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)))
    * [Web map viewer](https://www.arcgis.com/home/webmap/viewer.html?url=https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer&source=sd) (just for layers, not for tables)
    * [ArcGIS REST Service Page > Layer > Query](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/query)

#### Add, update and delete multiple records with a single call

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<catalog-url>/<serviceName>/FeatureServer/<layerId>/applyEdits`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/applyEdits](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/applyEdits)

This operation adds, updates, and deletes features to the associated feature layer or table in a single call.

Featured parameters ([view all](https://developers.arcgis.com/rest/services-reference/apply-edits-feature-service-layer-.htm)):

* `adds`: The array of features ([JSON Feature Objects](https://developers.arcgis.com/documentation/common-data-types/feature-object.htm)) to be added.
* `updates`: The array of features ([JSON Feature Objects](https://developers.arcgis.com/documentation/common-data-types/feature-object.htm)) to be updated
* `deletes`: Comma separated values of object IDs of the features/records to be deleted.

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/apply-edits-feature-service-layer-.htm).
* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)
* Sample GUIs using this endpoint:
    * [Item details page > Data](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1#data) ([Documentation]((https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)))
    * [Web map viewer](https://www.arcgis.com/home/webmap/viewer.html?url=https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer&source=sd) (Click on any feature > click edit > edit any field > click close)
    * [ArcGIS REST Service Page > Layer > Apply Edits](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0//applyEdits)


#### Delete records

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<catalog-url>/<serviceName>/FeatureServer/<layerId>/deleteFeatures`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/deleteFeatures](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/deleteFeatures)

This operation deletes records in a feature layer or table. A list of objectIds or a `where` clause can be used to specify which records we want to delete. We can also specify a spatial filter and much more

**Resources**:

* [Full documentation](https://developers.arcgis.com/rest/services-reference/delete-features.htm).
* [Add, edit, and remove features](https://developers.arcgis.com/labs/rest/add-edit-and-remove-features/) (tutorial)
* Sample GUIs using this endpoint:
    * [ArcGIS REST Service Page > Layer > Delete Features](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0//deleteFeatures)

#### Add attachments to a record

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Postman request name**: `<catalog-url>/<serviceName>/FeatureServer/<layerId>/<featureId>/addAttachment`<br>
> **Example**: [https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/1/addAttachment](https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0/1/addAttachment)

This operation adds an attachment to the associated feature and is available only if the layer has advertised that it has attachments. A layer has attachments if its hasAttachments property is true.

See the [Limiting upload file size and file types](https://developers.arcgis.com/rest/services-reference/uploads.htm) section under Uploads to learn more about default file size and file type limitations imposed on attachments.

* [Full documentation](https://developers.arcgis.com/rest/services-reference/add-attachment.htm).
* Sample GUIs using this endpoint:
    * [Web map viewer > Edit > Choose File > Close](https://www.arcgis.com/home/webmap/viewer.html?url=https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer&source=sd) (just for layers, not for tables)
    * [Item details page > Data > Add](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1#data) ([Documentation]((https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm)))
    * [ArcGIS REST Service Page > Layer > (add an id to the URL) > Add attachment](https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/NewFeatureService/FeatureServer/0/1/addAttachment)

### How to manage layer views (filter / extend tables)

#### Create a layer view

##### Content from individual layers

> **Go to**: [TOC](#table-of-contents) | [Quick reference](#quick-reference)

> **Note**: this operation does not requires any specific endpoint, it will use the existing ones.

If you need a different view of the data represented by a hosted feature layer—for example, you want to apply different editor settings, styles or filters—create a hosted feature layer view of that hosted feature layer. Once you have a view, you can [define which features or fields are available in the hosted feature layer view](https://doc.arcgis.com/en/arcgis-online/manage-data/set-view-definition.htm) and share the view to groups whose members need access to that view of the data.

When you create a feature layer view, a new hosted feature layer item is added to Content. This new layer is a view of the data in the hosted feature layer, which means updates made to the data appear in the hosted feature layer and all of its hosted feature layer views. However, since the view is a separate layer, you can change properties and settings on this item separately from the hosted feature layer from which it is created. For example, you can allow members of your organization to edit the hosted feature layer but share a read-only feature layer view with the public.

**Only the owner of a hosted feature layer can create a hosted feature layer view from the original layer**. This is different than copying a layer, which can be done by non-owners and even public users.


**Step 1) [Create an empty database](#how-to-create-an-empty-database)**

> We will use the endpoint `<root-url>/content/users/<username>/createService`

* `isView` = `true`
* `outputType` = featureService
* `createParameters`

```js
{
	"name": "NewFeatureService_LayerView",
	"isView": true,
	"sourceSchemaChangesAllowed": true,
	"isUpdatableView": true,
	"spatialReference": {
		"wkid": 102100,
		"latestWkid": 3857
	},
	"initialExtent": {
		"xmin": -20037507.0671618,
		"ymin": -30240971.9583862,
		"xmax": 20037507.0671618,
		"ymax": 18398924.324645,
		"spatialReference": {
			"wkid": 102100,
			"latestWkid": 3857
		}
	},
	"capabilities": "Query",
	"preserveLayerIds": false
}
```

**Step 2) Link to the source layer(s) and/or table(s) (add the new service definition)**

Now we are going to link it to the layer we want.

> **Note**: we will use the endpoint `<admin-catalog-url>/<serviceName>/FeatureServer/updateDefinition`

Example of an `addToDefinition` param:

```js
{
	"layers": [{
		"currentVersion": 10.7,
		"id": 0,
		"name": "Point layer",
		"type": "Feature Layer",
		"displayField": "",
		"description": "",
		"copyrightText": "",
		"defaultVisibility": true,
		"editingInfo": {
			"lastEditDate": 1570034310767
		},
		"isDataVersioned": false,
		"supportsAppend": true,
		"supportsCalculate": true,
		"supportsASyncCalculate": true,
		"supportsTruncate": false,
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
		"supportsReturningQueryGeometry": true,
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
			"supportsPercentileStatistics": true,
			"supportsQueryAttachments": true,
			"supportsLod": true,
			"supportsQueryWithLodSR": false,
			"supportedLodTypes": ["geohash"],
			"supportsReturningGeometryCentroid": false,
			"supportsQueryWithDatumTransformation": true,
			"supportsHavingClause": true,
			"supportsOutFieldSQLExpression": true,
			"supportsMaxRecordCountFactor": true,
			"supportsTopFeaturesQuery": true,
			"supportsDisjointSpatialRel": true,
			"supportsQueryWithCacheHint": true,
			"supportsQueryAttachmentsWithReturnUrl": true
		},
		"useStandardizedQueries": true,
		"geometryType": "esriGeometryPoint",
		"minScale": 0,
		"maxScale": 0,
		"extent": {
			"xmin": -20037508.342788905,
			"ymin": -8175201.3721496435,
			"xmax": -3.805489,
			"ymax": 12175461.54272524,
			"spatialReference": {
				"wkid": 102100,
				"latestWkid": 3857
			}
		},
		"drawingInfo": {
			"renderer": {
				"type": "simple",
				"symbol": {
					"type": "esriPMS",
					"url": "RedSphere.png",
					"imageData": "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",
					"contentType": "image/png",
					"width": 15,
					"height": 15
				}
			}
		},
		"allowGeometryUpdates": true,
		"hasAttachments": true,
		"attachmentProperties": [{
			"name": "name",
			"isEnabled": true
		}, {
			"name": "size",
			"isEnabled": true
		}, {
			"name": "contentType",
			"isEnabled": true
		}, {
			"name": "keywords",
			"isEnabled": true
		}, {
			"name": "exifInfo",
			"isEnabled": true
		}],
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
		"types": [],
		"templates": [{
			"name": "New Feature",
			"description": "",
			"drawingTool": "esriFeatureEditToolPoint",
			"prototype": {
				"attributes": {
					"NewField3": null
				}
			}
		}],
		"supportedQueryFormats": "JSON, geoJSON",
		"hasStaticData": false,
		"maxRecordCount": 2000,
		"standardMaxRecordCount": 32000,
		"tileMaxRecordCount": 8000,
		"maxRecordCountFactor": 1,
		"capabilities": "Create,Delete,Query,Update,Editing",
		"url": "https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0?token=QVaUAApYTUCCYslHwWKJ1_8rDKdprv6v8Q2hFVD4t-L8Krsr0WzQIO95xnj9kjNLJ6U-3aTcBnbPq8XZNntRzasjbOBosHc4FYiydVtWQUrbJ1UUrZJoc3KOwvf-SMUHNCrqLikIrqr8sj3hTRJexRMY_Wd0u4RrdowoyYJC1QGBoCiDVhRZQ6HHcK-FRjH4BaU1OV37nQwVYqRvzg8Ds0tD-F114bIFpjtcwkScxNM7KZvk_JetLEa-6yaEAY9i",
		"attributes": ["is-hosted", "has-no-dateTime", "can-appendData", "can-renameLayer"],
		"layerMetadataUrl": "",
		"mapViewerUrl": "https://awesome-arcgis.maps.arcgis.com/home/webmap/viewer.html?layers=0e3e6432498c4ef091cd58b1cffa064b&layerId=0",
		"mapViewerUrlWithGeocode": "./webmap/viewer.html?layers=0e3e6432498c4ef091cd58b1cffa064b&review=true&layerId=0",
		"sceneViewerUrl": "./webscene/viewer.html?layers=0e3e6432498c4ef091cd58b1cffa064b&layerId=0",
		"adminLayerInfo": {
			"viewLayerDefinition": {
				"sourceServiceName": "NewFeatureService",
				"sourceLayerId": 0,
				"sourceLayerFields": "*"
			}
		}
	}],
	"tables": []
}
```

As notice for **each layer and table** we will have to specify some special properties:

* `viewLayerDefinition`: specifying the unique sourceServiceName (e.g. `"NewFeatureService"`) each layer and table is referencing.
* `url`: URL to the original feature service (not item), notice is contains the sourceServiceName (`https://services7.arcgis.com/rF1wdZICHfgsvter/arcgis/rest/services/NewFeatureService/FeatureServer/0`)
* `mapViewerUrl`: link to the map viewer loading the original item
* `mapViewerUrlWithGeocode`: link to the map viewer loading the original item
* `sceneViewerUrl`: link to the scene viewer loading the original item

**Resources**:

* [Create hosted feature layer views](https://doc.arcgis.com/en/arcgis-online/manage-data/create-hosted-views.htm)
* Sample GUIs using this endpoint:
    * [Item details page > Create View Layer](https://awesome-arcgis.maps.arcgis.com/home/item.html?id=09d51c9fdd474d208b6c2f5fb523d1d1#data) ([Documentation]((https://doc.arcgis.com/en/arcgis-online/manage-data/item-details.htm))) (only the owner of a hosted feature service)

##### Content from joined layers

Transfer attributes from one layer or table to another based on spatial and attribute relationships. Optionally, statistics can be calculated for the joined features.

The best way to understand how to do this is by checking the folder "Join two tables" within the Postman collection. But to summarize these are the steps:

1. Create empty service specifying `isView: true`
2. Update a feature service definition by adding a new layer with `adminLayerInfo.viewLayerDefinition.table.relatedTables` specifying information about the query parameters.

Optionals:

3. Update de item to add information to the `properties` field about the processing job `analysis.arcgis.com/.../tasks/GPServer/JoinFeatures`
4. Add a JSON to the item's static resources with the tool and parameters used (in this case [Join features](https://developers.arcgis.com/rest/services-reference/join-features.htm))
5. Update de item to add information to the `properties` field about the completed job

**Resources**:

* Sample GUIs using this endpoint:
    * Map viewer > Analysis > Summarize > Join features: Check the "Create results as hosted feature layer view" ([documentation](](https://doc.arcgis.com/en/arcgis-online/analyze/join-features.htm)))

#### Filter information in a layer view

To control what data users see, the owner of a hosted feature layer view, or an administrator, can define what fields or features are available in the view. You can also limit the hosted feature layer view to a specific area by defining a spatial extent. These definitions are saved with the hosted feature layer view and allow you more control over what content people see. [More info](https://doc.arcgis.com/en/arcgis-online/manage-data/set-view-definition.htm).

**PENDING**

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

> **Note**: *none of them have support to every capability of the REST API, that's why we still think it's worth learning how to use it directly.*

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
