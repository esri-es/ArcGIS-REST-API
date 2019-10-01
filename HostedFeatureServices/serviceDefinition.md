# Service definition specification

This documentation aims to compile all the information you need to know to build a service definition specification by yourself.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Layer Object](#layer-object)
- [Edit fields info Object](#edit-fields-info-object)
- [Access control Object](#access-control-object)
- [Relationship Object](#relationship-object)
- [Archiving info Object](#archiving-info-object)
- [Advanced query capabilities Object](#advanced-query-capabilities-object)
- [Geometry properties Object](#geometry-properties-object)
- [Extent Object](#extent-object)
- [Spatial reference object](#spatial-reference-object)
- [Height model info Object](#height-model-info-object)
- [Drawing info Object](#drawing-info-object)
- [Unique ID field Object](#unique-id-field-object)
- [Field model Object](#field-model-object)
- [Time info Object](#time-info-object)
- [Template object](#template-object)
- [Type Object](#type-object)
- [Subtypes object](#subtypes-object)
- [Admin layer info Object](#admin-layer-info-object)
- [Full examples](#full-examples)
  - [Point layer](#point-layer)
  - [Polyline layer](#polyline-layer)
  - [Polygon layer](#polygon-layer)
  - [Table](#table)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

When using the [addToDefinition](https://developers.arcgis.com/rest/services-reference/add-to-definition-feature-service-.htm) endpoint a parameter called `addToDefinition` have to be included.

It looks like this:

```js
{
	"layers": [{...}],
    "tables": [{...}]
}
```

|Property|Type|Details|
|---|---|---|
|layers|Array[\<[Layer Object](#layer-object)\>]|This will add one layer per each object in the array.
|tables|Array[\<Table Object\>]|



## Layer Object

> Extracted mainly from the [feature layer service page](https://developers.arcgis.com/rest/services-reference/layer-feature-service-.htm) and the [Web Map Specification > layerDefinition](https://developers.arcgis.com/web-map-specification/objects/layerDefinition/).

|Property|Type|Details|
|---|---|---|
|currentVersion|Double|Numeric value indicating the server version of the layer // Added at 10.0 SP1|
|id|Int|Layer or table ID
|name|String|Layer or table name
|type|String|\<`Feature Layer` \| `Table`\>
|parentLayer|Int|Parent layer ID // Added at 10.6
|description|String|String value of the layer as defined in the map service.
|copyrightText|String|It can be set to empty string
|defaultVisibility|Boolean|Boolean value indicating whether the layer's visibility is turned on // Added at 10.1
|editingInfo|\<[Edit fields info Object](#edit-fields-info-object)\>| If present, specifies information about editing .Ex. `{"lastEditDate": null},` **lastEditDate** indicates the last time a layer was edited. This value gets updated every time the layer data is edited or when any of the layer properties change // Added at 10.1
|displayField|String|The name of the layer's primary display field. The value of this property matches the name of one of the fields of the layer.
|objectIdField|String|Indicates the name of the object ID field in the dataset. The ObjectID field is maintained by ArcGIS and guarantees a unique ID for each row in the table. More about: [Object Identifiers](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/arcgis-field-data-types.htm#GUID-3C98DA8B-8D97-4E9F-AA34-F69C737A9871)
|uniqueIdField|\<[Unique ID field Object](#unique-id-field-object)\>|Unique numeric field in the polygon feature service. [Unique identifier fields in database tables](https://pro.arcgis.com/en/pro-app/help/data/databases/unique-identifier-fields-in-database-tables.htm)
|globalIdField|String|A Global ID uniquely identify a feature or table row within a geodatabase and across geodatabases. It can be set to empty string. [Global Identifiers](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/arcgis-field-data-types.htm#GUID-97064FAE-B42E-4DC3-A5C9-9A6F12D053A8)
|typeIdField|String|A string containing the name of the field holding the type ID for the features, if types exist for the dataset. Each available type has an ID, and each feature's typeIdField can be read to determine the type for each feature. It can be set to empty string
|fields|Array[\<[Field model object](#field-model-object)\>]|Layer / table fields
|geometryField|[Field model object](#field-model-object)|Describes settings of the geometry field itself and includes the name, nullable, and editable sub-properties. Other sub-properties such as modelName may or may not be provided. It is possible to have a geometry field that is not editable. For features in layers where editable = false, the geometry values are system maintained and cannot be edited directly even by the data owner or administrator (e.g. utility network dirty area layers). This is different from the `.metryUpdates` property, which allows the service owner or administrator to control whether or not non-owner/non-administrator users can make geometry updates. Owners or administrators can make geometry updates even when `allowGeometryUpdates` is false as long as the geometry field is editable
|types|Array[\<[Type Object](#type-object)\>]|Layer / table sub-types. Can be an empty array // Added at 10.0 - if the layer has sub-types, they'll be included
|subtypeField|String| Is set to the name of the subtype field. If the layer does not have subtypes, it is set to empty string `("subtypeField": "")` // Added at 10.5
|defaultSubtypeCode|Int| A layer property that is set to the default subtype code if the layer has subtypes // Added at 10.5
|templates|Array[\<[Template object](#template-object)\>]|Layer / table templates - usually present when the layer / table has no types
|subtypes|Array[\<[Subtypes object](#subtypes-object)\>]|Is an array that describes the subtypes in a layer and is always included if the layer has subtypes. The domains in the types array will match the domains in the subtype array for layers that have a unique value renderer based on the subtype column.
|ownershipBasedAccessControlForFeatures|\<[Access control Object](#access-control-object)\>\|`null`|Object describing how can update, delete and query // Added at 10.1
|relationships|Array[\<[Relationship Object](#relationship-object)\>]|The Layer resource returns `relatedTableId`, `cardinality`, `role`, `keyField`, and `composite` for all relationships. In addition, the `relationshiptableId` and `keyFieldInRelationshipTable` properties are returned for attributed relationships only. [Benefits of relationship classes](http://desktop.arcgis.com/en/arcmap/10.3/manage-data/relationships/benefits-of-relationship-classes.htm)
|isDataVersioned|Boolean|Boolean value indicating whether the data is versioned. [Overview of versioning](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/overview-of-versioning-in-arcgis-pro.htm) // Added at 10.1
|syncCanReturnChanges|Boolean|Will be true if the data is versioned and has global IDs; this indicates the server has the ability to return changes only. If `syncCanReturnChanges` is false, the server will include all data for each layer in the response.
|isDataArchived|Boolean|Is true if a layer is archive enabled which allows it to support query with historicMoment. [What is archiving?](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/what-is-archiving-.htm) // Added at 10.6
|isDataBranchVersioned|Boolean|Is true when a layer references a feature class or table in an enterprise geodatabase that is branch-versioned. See [branch versioning](http://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/data-management-strategies.htm#ESRI_SECTION1_6FA2CFB5F9484FF096740D653C674B5D) in enterprise geodatabases // Added at 10.7
|isCoGoEnabled|Boolean|Is true if a layer has coordinate geometry enabled. [CoGo overview](http://desktop.arcgis.com/en/arcmap/10.3/manage-data/creating-new-features/an-overview-of-cogo.htm) // Added at 10.6
|supportsRollbackOnFailureParameter|Boolean|Will be `true` to indicate the support for the `rollbackOnFailure` parameter in edit operations (for example, apply edits, add features, update features, and delete features). The `supportsRollbackOnFailureParameter` will be true if the data is simple and non-versioned. If `supportsRollbackOnFailureParameter` is `false` , `rollbackOnFailure` will be true during edit operations such as `applyEdits` // Added at 10.1
|archivingInfo|\<[Archiving info object](#archiving-info-object)\>| // Added at 10.5
|supportsAppend|Boolean|
|supportsCalculate|Boolean|
|supportsTruncate|Boolean|
|supportsAttachmentsByUploadId|Boolean|
|supportsAttachmentsResizing|Boolean|
|supportsRollbackOnFailureParameter|Boolean|
|supportsStatistics|Boolean|// Added at 10.1
|supportsAdvancedQueries|Boolean|// Added at 10.1
|supportsValidateSql|Boolean|
|supportsCoordinatesQuantization|Boolean|// Added at 10.6.1
|supportsApplyEditsWithGlobalIds|Boolean|
|advancedQueryCapabilities|\<[Advanced query capabilities Object](#advanced-query-capabilities-object)\>|Advanced query capabilities of a layer // Added 10.3
|useStandardizedQueries|Boolean|Will be true to indicate that the layer requires the use of standardized queries. Learn more about [standardized queries](https://enterprise.arcgis.com/en/server/latest/administer/windows/about-standardized-queries.htm).
|geometryType|String|\<`esriGeometryPoint` \| `esriGeometryPolyline` \| `esriGeometryPolygon`\> // Property applicable to feature layers only
|geometryProperties|\<[Geometry properties Object](#geometry-properties-object)\>| Property applicable to feature layers only // Added at 10.7
|minScale|Int|Integer property used to determine the minimum scale at which the layer is displayed // Property applicable to feature layers only
|maxScale|Int|Integer property used to determine the maximum scale at which the layer is displayed // Property applicable to feature layers only
|extent|\<[Extent Object](#extent-object)\> \| `null`|An object defining the rectangular area.  // Property applicable to feature layers only
|heightModelInfo|\<[Height model info Object](#height-model-info-object)\>|The height model info defines the characteristics of a vertical coordinate system. [HeighModelInfo @ JS API doc](https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-HeightModelInfo.html)
|sourceSpatialReference|\<[Spatial reference Object](#spatial-reference-object)\>|Property describes the coordinate system of the feature class in the geodatabase // Added at 10.6
|sourceHeightModelInfo|\<[Height model info Object](#height-model-info-object)\>|Is a layer property that describes the feature classes vertical coordinate system when defined. Note 1) When querying, z values are returned in the sourceSpatialReference vertical coordinate system regardless of what is specified as the output spatial reference. It is also expected that z values are provided in the sourceSpatialReference source vertical coordinate system when editing. Note 2) The features listed above are available for non-hosted services published from ArcGIS Pro 2.1 or later but not necessarily for services published from ArcMap or other processes. Services published from ArcGIS Pro 2.1 have the following layer and service property: "cimVersion": "2.1.0" // Added at 10.6
|drawingInfo|\<[Drawing info Object](#drawing-info-object)\>|Contains the drawing and labeling information.
|hasM|Boolean|If the features in the layer have M values, the `hasM` property will be true
|hasZ|Boolean|If the features in the layer have Z values, the `hasZ` property will be true
|enableZDefaults|Boolean|If the layer / table supports querying based on time // Added at 10.1
|zDefault|Double|If the layer / table supports querying based on time // Added at 10.1
|allowGeometryUpdates|Boolean|Allows the service owner or administrator to control whether or not non-owner/non-administrator users can make geometry updates. Owners or administrators can make geometry updates even when `allowGeometryUpdates` is false as long as the geometry field is editable// Added at 10.1
|timeInfo|\<[Time info Object](#time-info-Object)\>|The time info metadata of the layer. May be set for feature layers inside a feature collection item.
|hasAttachments|Boolean|If the layer / table has attachments, the `hasAttachments` property will be true
|htmlPopupType|String|\<`esriServerHTMLPopupTypeNone` \| `esriServerHTMLPopupTypeAsURL` \| `esriServerHTMLPopupTypeAsHTMLText`\> // from 10 onward - indicates whether the layer / table has htmlPopups
|maxRecordCount|Int|Maximum number of records that will be returned at once for a query // Added at 10.1
|standardMaxRecordCount|Int|Maximum number of features a query will return when the query uses resultType = standard // Added at 10.6.1
|tileMaxRecordCount|Int|Maximum number of features a query will return when the query uses resultType = tile // Added at 10.6.1
|maxRecordCountFactor|Int|Is used to change the values of standardMaxRecordCount and tileMaxRecordCount for querying // Added at 10.6.1
|supportedQueryFormats|String| Describes the supported response types when querying a feature service layer. Values include json, html, and in 10.7 may also include pbf (protocol buffer), a compact binary encoding for geographic data. Ex. `"JSON, geoJSON, PBF"` // Added at 10.1
|hasMetadata|Boolean|Indicates whether a layer contains metadata // Added at 10.6.1
|hasStaticData|Boolean|Boolean value indicating whether data changes. True if it does not.
|sqlParserVersion|String|Is added for hosted feature service layers to indicate the supported SQL 92 syntax for standardized queries. Values include “PG_10.6.1” for relational data store based hosted feature services and “ES_10.6.1” for hosted feature services based in a spatiotemporal ArcGIS Data store. No new query operations have been added for the 10.7 release, which means that all query operations as of the 10.7 release fall under sqlParserVersion 10.6.1. Most of the SQL 92 syntax for standardized queries is supported with relational data store based hosted feature services. Hosted feature services in spatiotemporal ArcGIS Data Stores do support a subset – see the [layer query operation](https://developers.arcgis.com/rest/services-reference/query-feature-service-layer-.htm) where clause help for more information. Ex. `"PG_10.6.1"`// Added at 10.7
|isUpdatableView|Boolean|Is true on a hosted feature service view layer when service definition updates (e.g. enabling and disabling capabilities) are allowed on the view layers.
|capabilities|String|Comma separated. Ex. `"Query,Editing,Create,Update,Delete,Sync"`,
|indexes|Array|Can be an empty array. [Manage hosted feature layers > Rebuild the spatial index](https://doc.arcgis.com/en/arcgis-online/manage-data/manage-hosted-feature-layers.htm#ESRI_SECTION1_C0E9631E20E245CA96784EC95393BD7D) | [Add Attribute Index using ArcGIS Pro](https://pro.arcgis.com/en/pro-app/tool-reference/data-management/add-attribute-index.htm)
|syncEnabled|Boolean|When the Sync capability is listed, the feature service is sync-enabled, and all layers and tables in the service can be used in sync workflows. [Sync-enabled feature services](https://developers.arcgis.com/rest/services-reference/sync-enabled-feature-services.htm) \| [Manage hosted feature layers](https://doc.arcgis.com/en/arcgis-online/manage-data/manage-hosted-feature-layers.htm)
|adminLayerInfo|\<[Admin layer info Object](#admin-layer-info-object)\>|If name property of the layer is the not the name of the table in the database, the tableName property is used to define the fully qualified table name in the database.

## Edit fields info Object

```js
{
    "creationDateField": "<creationDateField>",
    "creatorField": "<creatorField>",
    "editDateField": "<editDateField>",
    "editorField": "<editorField>",
    "realm": "<realm>",
    // Added at 10.7
    "dateFieldsTimeReference": {
        "timeZone": "<timeZone>",
        "respectsDaylightSaving": < true | false >
    }
}
```

Example:

```js
{
    "creationDateField": "created_date",
    "creatorField": "created_user",
    "editDateField": "last_edited_date",
    "editorField": "last_edited_user",
    "dateFieldsTimeReference": {
        "timeZone": "UTC",
        "respectsDaylightSaving": false
     }
}
```

## Access control Object

```js
{
   "allowOthersToUpdate": < true | false > ,
   "allowOthersToDelete": < true | false > ,
   "allowOthersToQuery": < true | false >
}
```

Example:

```js
{
    "allowOthersToQuery": true
}
```

## Relationship Object


This property specifies if the table / layer is related to another table / layer of the same service:

* [Overview of the relationship class toolset in ArcGIS Pro (Desktop app)](https://pro.arcgis.com/en/pro-app/tool-reference/data-management/an-overview-of-the-relationship-classes-toolset.htm)
    * Parameters: [Usage and syntax](https://pro.arcgis.com/en/pro-app/tool-reference/data-management/create-relationship-class.htm#ESRI_USAGES_6BBF052A9B4B4D91A788AAD83D0521D7)
* Demo: [Publish a really simple database within ArcGIS Online using ArcGIS Pro](https://www.youtube.com/watch?v=wKut-_f3C10)

```js
{
    "id": < relationshipId1 > ,
    "name": "<relationshipName1>",
    "relatedTableId": < relatedTableId1 > ,
    "cardinality": "<esriRelCardinalityOneToOne>|<esriRelCardinalityOneToMany>|<esriRelCardinalityManyToMany>";, // Added at 10.1
    "role": "<esriRelRoleOrigin>|<esriRelRoleDestination>";, // Added at 10.1
    "keyField": "<keyFieldName2>", // Added at 10.1
    "composite": < true > | < false > , // Added at 10.1
    "relationshipTableId": < attributedRelationshipClassTableId > , // Added in 10.1. Returned only for attributed relationships
    "keyFieldInRelationshipTable": "<key field in AttributedRelationshipClass table that matches keyField>" // Added in 10.1. Returned only for attributed relationships
}
```

Example: (from [this layer](https://services9.arcgis.com/JAEHQgpk7zTHeZED/ArcGIS/rest/services/Best_Practices_WFL1/FeatureServer/1?f=json))

```js
{
    "id": 0,
    "name": "Sites - BP",
    "relatedTableId": 0,
    "cardinality": "esriRelCardinalityOneToOne",
    "role": "esriRelRoleDestination",
    "keyField": "Latitude",
    "composite": false
}
```

## Archiving info Object

* `supportsQueryWithHistoricMoment` indicates whether historic moment queries can be performed on the layer. A layer must be archiving-enabled to support these type of queries.
* `startArchivingMoment` indicates the time archiving was enabled on the layer.

```js
{
    "supportsQueryWithHistoricMoment": < true | false > ,
    "startArchivingMoment": < startArchivingMoment >
}
```

Example:
```js
{
    "supportsQueryWithHistoricMoment": false,
    "startArchivingMoment": -1
}
```

## Advanced query capabilities Object

* `supportsPagination` will be true if the layer supports pagination in the query operation.
* `supportsQueryWithDistance` will be true if the layer supports providing a distance parameter in the query operation.
* `supportsReturningQueryExtent` will be true if the layer supports returnExtentOnly in the query operation.
* `supportsStatistics` indicates whether the layer supports statistical functions in a query operation.
* `supportsOrderBy` will be true if the layer supports the orderByFields parameter in the query operation.
* `supportsDistinct` will be true if the layer supports the returnDistinctValues parameter in the query operation.

```js
{
    "supportsPagination":  < true | false > ,
    "supportsTrueCurve": < true | false > ,
    "supportsPaginationOnAggregatedQueries":  < true | false > ,
    "supportsQueryRelatedPagination":  < true | false > ,
    "supportsQueryWithDistance":  < true | false > ,
    "supportsReturningQueryExtent":  < true | false > ,
    "supportsStatistics":  < true | false > ,
    "supportsOrderBy":  < true | false > ,
    "supportsDistinct":  < true | false > ,
    "supportsQueryWithResultType":  < true | false > , // Added at 10.6.1
    "supportsSqlExpression":  < true | false > ,
    "supportsAdvancedQueryRelated":  < true | false > ,
    "supportsCountDistinct":  < true | false > , // Added at 10.6.1
    "supportsLod":  < true | false > ,
    "supportsReturningGeometryCentroid":  < true | false > , // Added at 10.6.1
    "supportsQueryWithDatumTransformation":  < true | false > ,
    "supportsHavingClause":  < true | false > , // Added at 10.6.1
    "supportsOutFieldSQLExpression":  < true | false > ,
    "supportsTopFeaturesQuery":  < true | false >, // Added at 10.7   
    "supportsOrderByOnlyOnLayerFields":  < true | false >, // Added at 10.7   
    "supportsQueryAttachments":  < true | false >, // Added at 10.7   
    "supportsQueryAttachmentsWithReturnUrl":  < true | false > // Added at 10.7   
}
```

Example:

```js
{
    "supportsPagination": true,
    "supportsTrueCurve": true,
    "supportsQueryWithDistance": true,
    "supportsReturningQueryExtent": true,
    "supportsStatistics": true,
    "supportsOrderBy": true,
    "supportsDistinct": true
}
```

## Geometry properties Object

```js
{
    "shapeAreaFieldName": "<shapeAreaFieldName>",
    "shapeLengthFieldName": "<shapeLengthFieldName>",
    "units": "<esriFeet | esriKilometers | esriMeters | esriMiles>"
}
```

Example:

```js
{
    "shapeAreaFieldName" : "Shape__Area",
    "shapeLengthFieldName" : "Shape__Length",
    "units" : "esriMeters"
}
```

## Extent Object

This object defines the bounding geometry given the lower-left and upper-right corners of the bounding box.

```js
{
    "xmin": < xmin > ,
    "ymin": < ymin > ,
    "xmax": < xmax > ,
    "ymax": < ymax > ,
    "spatialReference": <Spatial reference object>
}
```

Example:

```js
{
    "xmin": -122.514435102,
    "ymin": 5.6843418860808E-14,
    "xmax": 138.625776397,
    "ymax": 67.1577965990001,
    "spatialReference": {
        "wkid": 4326,
        "latestWkid": 3857,
        "vcsWkid": 5702,
        "latestVcsWkid": 5702,
        "xyTolerance": 0.001,
        "zTolerance": 0.001,
        "mTolerance": 0.001,
        "falseX": -20037700,
        "falseY": -30241100,
        "xyUnits": 10000,
        "falseZ": -100000,
        "zUnits": 10000,
        "falseM": -100000,
        "mUnits": 10000
    }
}
```

## Spatial reference object

```js
{
    "wkid": < wkid > ,
    "latestWkid": < latestWkid > ,
    // Added at 10.6 when map is published with a vertical coordinate system
    "vcsWkid": < vcsWkid > ,
    "latestVcsWkid": < latestVcsWkid > ,
    "xyTolerance": < xyTolerance > ,
    "zTolerance": < zTolerance > ,
    "mTolerance": < mTolerance > ,
    "falseX": < falseX > ,
    "falseY": < falseY > ,
    "xyUnits": < xyUnits > ,
    "falseZ": < falseZ > ,
    "zUnits": < zUnits > ,
    "falseM": < falseM > ,
    "mUnits": < mUnits >
}
```

Example:

```js
{
    "wkid": 4326,
    "latestWkid": 3857,
    "vcsWkid": 5702,
    "latestVcsWkid": 5702,
    "xyTolerance": 0.001,
    "zTolerance": 0.001,
    "mTolerance": 0.001,
    "falseX": -20037700,
    "falseY": -30241100,
    "xyUnits": 10000,
    "falseZ": -100000,
    "zUnits": 10000,
    "falseM": -100000,
    "mUnits": 10000
}
```

## Height model info Object

> Documentation from: [Web Scene Specification > heightModelInfo](https://developers.arcgis.com/web-scene-specification/objects/heightModelInfo/) & [JS API 4.x documentation > HeightModelInfo](https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-HeightModelInfo.html#properties-summary)


```js
{
    "heightModel": "<ellipsoidal|gravity_related_height>",
    "vertCRS": "<vertCRS>",
    "heightUnit": "<150-kilometers | 50-kilometers | benoit-1895-b-chain | clarke-foot | clarke-link | clarke-yard | foot | gold-coast-foot | indian-1937-yard | indian-yard | meter | sears-1922-truncated-chain | sears-chain | sears-foot | sears-yard | us-foot>"
}
```

Example:

```js
{
    "heightModel": "gravity_related_height",
    "vertCRS": "NGVD_1929",
    "heightUnit": "us-foot"
}
```

## Drawing info Object

* [renderer](https://developers.arcgis.com/web-map-specification/objects/renderer/): An object defined which provides the symbology for the layer.
* transparency: Number value ranging between 0 (no transparency) to 100 (completely transparent).
* [labelingInfo](https://developers.arcgis.com/web-map-specification/objects/labelingInfo/): An object defining the properties used for labeling the layer

```js
{
    "renderer": < renderer > ,
    "transparency": < transparency > ,
    "labelingInfo": < labelingInfo >
}
```

Example:

```js
{
	"renderer": {
		"type": "uniqueValue",
		"field1": "req_type",
		"field2": null,
		"field3": null,
		"defaultSymbol": null,
		"defaultLabel": "\u003call other values\u003e",
		"uniqueValueInfos": [{
				"value": "Blocked Street or Sidewalk",
				"label": "Blocked Street or Sidewalk",
				"description": "",
				"symbol": {
					"type": "esriPMS",
					"url": "1DD4FC53",
					"imageData": "iVBORw0KGgoAAAANSUhEUgAAABoAAAAaCAYAAACpSkzOAAAAAXNSR0IB2cksfwAAAAlwSFlzAAAOxAII=",
					"contentType": "image/png",
					"color": null,
					"width": 19,
					"height": 19,
					"angle": 0,
					"xoffset": 0,
					"yoffset": 0
				}
			},
			{
				"value": "Damaged Property",
				"label": "Damaged Property",
				"description": "",
				"symbol": {
					"type": "esriPMS",
					"url": "DF3100A6",
					"imageData": "iVBORw0KGgoAAAANSUhEUgAAABQAAAAMCAYAAABiDJ37AAAAAXNSR0IB2cksfwAAAAlwSFlzAAAOxAII=",
					"contentType": "image/png",
					"color": null,
					"width": 15,
					"height": 9,
					"angle": 0,
					"xoffset": 0,
					"yoffset": 0
				}
			}
		]
	},
	"transparency": 0,
	"labelingInfo": null
}
```

> **Tip**: the fastest way to create a drawing info object is by using the [Web Map Viewer](https://esri-es.github.io/awesome-arcgis/arcgis/products/web-map-viewer/) or [Web Scene Viewer](https://esri-es.github.io/awesome-arcgis/arcgis/products/web-scene-viewer/) to authorize a [web map](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/web-map/) or [web scene](https://esri-es.github.io/awesome-arcgis/esri/open-vision/open-specifications/web-scene/) and afterwards copying the drawingInfo object from there.


## Unique ID field Object

```js
{
    "name": "<fieldName>",
    "isSystemMaintained": <true | false>
}
```

Example:

```js
{
    "name": "OBJECTID",
    "isSystemMaintained": true
}
```

## Field model Object

> [What's New in 10.6 - New Properties Exposed on the Feature Layer](https://www.esri.com/arcgis-blog/products/announcements/announcements/whats-new-in-10-6-new-properties-exposed-on-the-feature-layer/)

```js
{
    "name": "<fieldName1>",
    "type": "<fieldType1>",
    "alias": "<fieldAlias1>",
    "domain": < domain1 > ,
    "editable": <true | false>,
    "nullable": <true | false>,
    "length": "<length1>",
    "defaultValue": "<defaultValue1>",
    "modelName": "<modelName1>"
}
```

Example:

```js
{
    "name": "objectid",
    "type": "esriFieldTypeOID",
    "alias": "Object ID",
    "editable": false,
    "nullable": true,
    "domain": null,
    "defaultValue": null,
    "modelName": "OBJECTID"
}
```

> While defining the "fields" property in a layer it might include a "sqlType" property. [Find more "sqlType" values](https://developers.arcgis.com/rest/services-reference/layer-feature-service-.htm#UL_01B2204C60864812A6E013ACD589E881)

Example 2:

```js
{
    "name" : "Shape__Area",
    "type" : "esriFieldTypeDouble",
    "alias" : "Shape__Area",
    "sqlType" : "sqlTypeDouble",
    "nullable" : true,
    "editable" : false,
    "domain" : null,
    "defaultValue" : null
}
```

## Time info Object

> Information from [Web map specification > layerTimeInfo](https://developers.arcgis.com/web-map-specification/objects/layerTimeInfo/)

* `startTimeField`: The name of the attribute field that contains the start time information.
* `endTimeField`: The name of the attribute field that contains the end time information.
* `trackIdField`: The field that contains the trackId.
* `timeExtent`: The time extent for all the data in the layer.
* [timeReference](https://developers.arcgis.com/web-map-specification/objects/timeReference/): Information about how the time was measured.
* `timeInterval`: Time interval of the data in the layer. Typically used for the TimeSlider when animating the layer.
* `timeIntervalUnits`: Temporal unit in which the time interval is measured.

```js
{
    "startTimeField": "<startTimeFieldName>",
    "endTimeField": "<endTimeFieldName>",
    "trackIdField": "<trackIdFieldName>",
    "timeExtent": [ < startTime > , < endTime > ],
    "timeReference": {
    	"timeZone": "<timeZone>",
    	"respectsDaylightSaving": < true | false >
    },
    "timeInterval": < timeInterval > ,
    "timeIntervalUnits": "<esriTimeUnitsUnknown | esriTimeUnitsCenturies | esriTimeUnitsDays | esriTimeUnitsDecades | esriTimeUnitsHours | esriTimeUnitsMilliseconds | esriTimeUnitsMinutes | esriTimeUnitsMonths | esriTimeUnitsSeconds | esriTimeUnitsWeeks | esriTimeUnitsYears>"
}
```

Example ([from this item](https://www.arcgis.com/home/item.html?id=66226c920b7d432f99730d9ab007c63b)):

```js
{
    "startTimeField": "DATE_START",
    "endTimeField": "DATE_END",
    "trackIdField": "",
    "timeExtent": [631238400000, 1609372800000],
    "timeReference": {
        "timeZone": "UTC",
        "respectsDaylightSaving": false
    },
    "timeInterval": 5,
    "timeIntervalUnits": "esriTimeUnitsYears",
    "exportOptions": {
        "useTime": false,
        "timeDataCumulative": false,
        "TimeOffset": 0,
        "timeOffsetUnits": "esriTimeUnitsCenturies"
    },
    "hasLiveData": false
}
```

## Template object

> More info at [Web map specification > Template](https://developers.arcgis.com/web-map-specification/objects/template/)

```js
{
    "name": "<templateName1>",
    "description": "<templateDescription1>",
    "prototype": < prototypicalFeature1 >,
    "drawingTool": "esriFeatureEditToolPoint"
}
```

Example:

```js
{
    "name": "Graffiti Complaint",
    "description": "",
    "drawingTool": "esriFeatureEditToolPoint",
    "prototype": {
        "attributes": {
            "status": 1,
            "req_id": null,
            "req_type": "Graffiti Complaint - Private Property",
            "req_date": null,
        }
    }
}
```

## Type Object

More information about the:

* [domain specification](https://developers.arcgis.com/web-map-specification/objects/domain/)
* [Template Object](#template-object)

```js
{
    "id": < typeId1 > ,
    "name": "<typeName1>",
    "domains": {
        "<domainField11>": < domain11 > ,
        "<domainField12>": < domain12 > ,
        "description": "<domainDescription>" // Added in 10.6
    },
    "templates": [{
            "name": "<templateName11>",
            "description": "<templateDescription11>",
            "prototype": < prototypicalFeature11 >
        },
        {
            "name": "<templateName12>",
            "description": "<templateDescription12>",
            "prototype": < prototypicalFeature12 >
        }
    ]
}
```

Example:

```js
{
    "id": "Graffiti Complaint - Private Property",
    "name": "Graffiti Complaint",
    "domains": {
        "description": null
    },
    "templates": [{
        "name": "Graffiti Complaint",
        "description": "",
        "drawingTool": "esriFeatureEditToolPoint",
        "prototype": {
            "attributes": {
                "status": 1,
                "req_id": null,
                "req_type": "Graffiti Complaint - Private Property",
                "req_date": null,
            }
        }
    }]
}
```


## Subtypes object

> More information about the [domain specification](https://developers.arcgis.com/web-map-specification/objects/domain/)

```js
{
    "code": < SubtypeCode1 > ,
    "name": "<SubtypeDescription1>",
    "defaultValues": {
        "<fieldName1>": < default1 > ,
        "<fieldName2>": "<default2>"
    },
    "domains": {
        "<fieldName1>": < domain11 > ,
        "<fieldName2>": < domain12 >
    }
}
```

Example:

```js
{
    "code": 1,
    "name": "subtype1",
    "defaultValues": {
        "FLD0_SUBT_FC2": 1,
        "FLD1_LONG_FC2": 25,
        "FLD2_DBL_FC2": null,
        "FLD3_DBL_FC2": 1000.1,
        "FLD4_TEXT_FC2": "code 200"
    },
    "domains": {
        "FLD1_LONG_FC2": {
            "type": "inherited"
        },
        "FLD3_DBL_FC2": {
            "type": "codedValue",
            "name": "CDOM_4",
            "codedValues": [{
                    "name": "coded 1000.1 desc",
                    "code": 1000.1
                },
                {
                    "name": "coded 2000.1 desc",
                    "code": 2000.2
                },
                {
                    "name": "coded 3000.1 desc",
                    "code": 3000.3
                }
            ],
            "mergePolicy": "esriMPTDefaultValue",
            "splitPolicy": "esriSPTDefaultValue"
        },
        "FLD4_TEXT_FC2": {
            "type": "inherited"
        }
    }
}
```

## Admin layer info Object

More information at [Feature Service Object](http://resources.arcgis.com/en/help/sds/rest/featureServiceObject.html)

```js
{
    "tableName": "<tableName>",
    "geometryField": <Field model Object>,
    "tableExtent": <Extent Object>
}
```

Example:

```js
{
    "tableName": "demo.dbo.WorldCities",
    "geometryField": {
        "name": "Shape"
    },
    "tableExtent": {
        "xmin": -179.270004272,
        "ymin": -79.150001526,
        "xmax": 177.130187988,
        "ymax": 78.199996948,
        "spatialReference": {
            "wkid": 4326,
            "wkt": "GEOGCS[\"GCS_WGS_1984\",DATUM[\"D_WGS_1984\",SPHEROID[\"WGS_1984\",6378137.0,298.257223563]],PRIMEM[\"Greenwich\",0.0],UNIT[\"Degree\",0.0174532925199433]]",
            "sdesrid": 4326
        }
    }
}
```

----

## Full examples

### Point layer

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

### Polyline layer

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

### Polygon layer

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

### Table

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
		".metryUpdates": true,
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
