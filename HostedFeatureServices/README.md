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
  - [How to manage database options](#how-to-manage-database-options)
    - [Visibility](#visibility)
    - [Edition, manage indexes, cache control and allow export data](#edition-manage-indexes-cache-control-and-allow-export-data)
- [Working with tables (Feature Layers)](#working-with-tables-feature-layers)
  - [How to manage tables](#how-to-manage-tables)
    - [Create a table](#create-a-table)
      - [Example of point layer](#example-of-point-layer)
      - [Example of polyline layer](#example-of-polyline-layer)
      - [Example of polygon layer](#example-of-polygon-layer)
      - [Example of standalone table](#example-of-standalone-table)
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

Checks whether a given service name and type are available for publishing a new service. true indicates that the name and type is not found in the organization's services and is available for publishing. false means the requested name and type are not available.

**Read**: [full documentation](https://developers.arcgis.com/rest/users-groups-and-items/check-service-name.htm)

### How to create an empty database

> **Endpoint name**: [\<root-url\>/content/users/\<username\>/createService](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm) <br>
> **Example**: https://www.arcgis.com/sharing/rest/content/users/awesome_arcgis/createService

The Create Service operation allows users to create a hosted feature service. You can use the API to create an empty hosted feature service from a feature service metadata JSON.

**Read**: [full documentation](https://developers.arcgis.com/rest/users-groups-and-items/create-service.htm)

### How to get database info

> **Endpoint name**: [\<catalog-url\>/\<serviceName\>/FeatureServer](https://developers.arcgis.com/rest/services-reference/feature-service.htm)<br>
> **Example**: [
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json](
https://services7.arcgis.com/rF1wdZICHfgsvter/ArcGIS/rest/services/Testing_purposes_POSTMAN_Collection/FeatureServer?f=json)

This resource provides basic information about the feature service, including the feature layers and tables that it contains, the service description, and so on.

**Read**: [full documentation](https://developers.arcgis.com/rest/services-reference/feature-service.htm)

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

## Working with tables (Feature Layers)

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
					"imageData": "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",
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

##### Example of standalone table

```js
{
	"layers": [{
		"currentVersion": 10.51,
		"id": 0,
		"name": "Standalone_table",
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

* [ArcGIS REST API Youtube Playlist](https://www.youtube.com/watch?v=omPiCTXZ0U8&list=PLahIW2YFPQd7o6L9fxSAXCuv5FldGkycP)

Other written resources:

* [Esri Open Vision > Open Specs > ArcGIS REST API](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/arcgis-rest-api/)
