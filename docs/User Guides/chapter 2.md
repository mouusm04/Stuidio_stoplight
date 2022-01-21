

KM Metadata Services

API Documentation

Version 1.0.0

14 January 2022

# Proprietary Rights

The information contained in this document is proprietary and confidential to
BlackSwan Technologies This material may not be duplicated, published, or
disclosed, in whole or in part, without the prior written permission of
BlackSwan Technologies.

# Summary of Changes** **

| **Version** | **Description** |
|-------------|-----------------|
| 1.0.0       | Initial Draft   |

# Table of Contents

[Proprietary Rights](#proprietary-rights)

[Summary of Changes](#summary-of-changes)

[Table of Contents](#table-of-contents)

[Chapter 1: Introduction](#chapter-1-introduction)

[What is Knowledge Mesh (KM) Metadata services?](#_Toc93136756)

[Metadata Model](#_Toc93136757)

[Examples](#_Toc93136758)

[View domains, data sources and terms](#view-domains-data-sources-and-terms)

[Create an app](#create-an-app)

[Create app schema](#create-app-schema)

[Import data sources from domain to
application](#import-data-sources-from-domain-to-application)

[Update attribute credibilities of data sources added to
applications](#update-attribute-credibilities-of-data-sources-added-to-applications)

[Chapter 2 Getting Started](#_Toc93136764)

[Authorization and Authentication](#_Toc93136765)

[How to Configure Users and Roles on Specific Cloud
IDPs](#how-to-configure-users-and-roles-on-specific-cloud-idps)

[Setting up BST Access Management
Service](#setting-up-bst-access-management-service)

[Make your First Call](#make-your-first-call)

[API Request and Response](#_Toc93136769)

[Abbreviations and Terms](#abbreviations-and-terms)

[Chapter 3 API Reference](#chapter-3-api-reference)

[Domain Knowledge Model](#domain-knowledge-model)

[Details of a knowledge model](#details-of-a-knowledge-model)

[Domains](#domains)

[List of available domains](#list-of-available-domains)

[Details of a specific domain](#details-of-a-specific-domain)

[Applications](#_Toc93136777)

[List of Registered Application in the
System](#list-of-registered-application-in-the-system)

# Chapter 1: Introduction

This chapter will provide an overview of KM metadata services, as well as
various metadata models and examples.

[What is Knowledge Mesh (KM) Metadata services? 6](#_What_is_Knowledge)

[Metadata model 6](#metadata-model)

[Examples 7](#examples)

## 

## 

## What is Knowledge Mesh (KM) Metadata services?

The purpose of this service is to provide an API that enables developers to
access domain metadata and define application metadata using the domain metadata
or by introducing application specific metadata on top of a predefined metadata
model.

These abilities can be used in order to support application runtime use cases
such as serving the user interface and obtaining metadata such as fetchers and
data transformation mappings during ingestion operations.

The relative position of KM SDK in the Element SDK is depicted on the conceptual
diagram below:

![](media/f457089517255a53c002d9559bf751d2.png)

## Metadata Model

The following section describes the different metadata resources and what they
represent.

| **Metadata Resources**       | **Description**                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Domain                       | A KM domain, such as "healthcare" or "company research".                                                                                                             |
| Domain Knowledge Model (DKM) | A default KG schema of a KM domain.                                                                                                                                  |
| Application                  | An application created by a user in a certain domain.                                                                                                                |
| Agent                        | Any agent involved in the KM, acting as an owner, creator, provider etc. of different resources.                                                                     |
| Data Catalog                 | The catalog of data sources associated with a specific domain.                                                                                                       |
| Data Source                  | A KM data source available to applications.                                                                                                                          |
| Application Data Source      | A data source connected to an application. This could be provided by the platform (in which case it should reference a data source owned by BST) or by application.  |
| Fetcher                      | A service providing access to a (fragment of a) dataset. It exposes some data querying interface and serves results in a specific format.                            |
| Data Mapping                 | A formal specification of a mapping from a data source (as served by a fetcher) to a DKM.                                                                            |
| Term                         | An individual term (entity/relationship/attribute type or a taxonomy concept), such as "Person", "worksFor", "Trojan technique".                                     |
| Application Term             | An individual term (entity/relationship/attribute type or a taxonomy concept), used in an application schema.                                                        |
| Attribute Matcher            | An attribute matcher                                                                                                                                                 |
| Application Schema           | A KG (Knowledge Graph) schema of an application.                                                                                                                     |

## 

## Examples

This section covers some common use cases where the KM Metadata Service gets
involved.

### View domains, data sources and terms

The following endpoints can be utilized with the relevant query parameters
(refer to the API Reference) to view domain, data sources and terms.

| Purpose                              | Endpoint                            |
|--------------------------------------|-------------------------------------|
| View list of domains                 | /domains                            |
| View list of datasources of a domain | /data_sources?domain_id={domain_id} |
| View terms of a domain               | /terms?domain_id={domain_id}        |

### Create an app

The following diagram indicates the endpoint to be invoked when registering an
application metadata resource with knowledge mesh. As shown in the diagram, when
an application metadata resource is created, an empty application schema
resource for the application is also created automatically.

![](media/aa124e1198a90e410f7fcde80a9c3888.png)

### Create app schema

The application schema is a collection of terms (Entities, Attributes and
Relationships) that define the application’s data model. The following diagram
illustrates the sequence of API calls involved in the typical process of
creating an application schema:

1.  First, the terms of the domain (i.e. organization research) for a given
    application are obtained.

2.  The required / suitable list of terms is chosen from the domain, renamed,
    weighted, and even application specific terms are created if required by the
    user / your application.

3.  Finally, the list of terms is added to the app schema, keeping in mind to
    indicate the target reference correctly for terms imported from the domain.
    In other words, these new terms are added to the app schema, any terms that
    were imported from the domain should indicate the ID of the domain term as
    the target reference.

![](media/c33a337e93d7c08fedf701dc5633c40e.png)

### 

### Import data sources from domain to application

### 

The knowledge mesh metadata comes with predefined data sources with fetchers and
mappings targeting various domains within the knowledge mesh. This example
highlights how these data sources can be utilized within an application.

1.  Get a list of data sources for the given domain of the application.

2.  Pick the data sources you wish to use by looking at the description, website
    etc.

3.  Add additional application specific metadata to the data source such as the
    KG (Knowledge Graph) usage category (the context in which this source is to
    be treated by KG operations), data validity period (frequency at which data
    should be refreshed) and custom tags (additional information you would like
    to store about the data source that you would like to include, that is not
    part of the predefined metadata mode).

4.  Add the datasource to the application.

![](media/9b8294db1f1b02e8645fd68b7f5dbdf5.png)

### Update attribute credibilities of data sources added to applications

Each data source added to an application can be configured in terms of the
credibility it represents in the context of a given domain, entity type term and
attribute type term. This example explains how this can be achieved.

The key concept to understand here is the Data Mapping metadata resource. The
Data Mapping is what has information about a data source in the context of a
domain and entity type term. In this respect, it also contains the information
related to the credibility of a given source for a given domain, entity type
term and even the attributes of this entity type. Therefore, setting the
attribute level / overall credibility of an app data source in the context of a
domain and entity type term is achieved by updating the data mapping.

![](media/6d3d6f3565edc6c0e4a8f60a2c12c39f.png)

# Chapter 2 Getting Started

In this chapter you will learn about authorization and authentication, configure
different user role, and how to use the Knowledge Mesh metadata service API to
send your first API call.

[Authorization and Authentication](#_Toc93136765)

[Make your First Call](#make-your-first-call)

[API Request and Response](#_Toc93136769)

[Abbreviations and Terms](#abbreviations-and-terms)

### Authorization and Authentication

The KM Metadata API supports bearer token-based authentication, which must be
included in the Authorization header of every request.

In order to authenticate such tokens and obtain authorization data, it leverages
another BST service named the “Access Management Service”.

Access Management Service should be deployed with access to the relevant cloud
IDP.

The following diagram illustrates this process at a very high level.

![](media/a25184f9c7653d47b993051ee15a232c.png)

The access management plays 2 roles:

1.  Abstracts the actual cloud IDP by communicating with multiple cloud IDPs,
    and shielding the consumer service from the underlying communication.

2.  Contains information that maps client roles defined in the IDP end with
    service specific permissions.

Therefore, in order to handle authentication and authorization, the following
steps need to be carried out.

1.  Setting up a user and assigning the required roles to that user within the
    IDP.

2.  Mapping the above created roles to service specific permissions within the
    BST Access Management Service.

    The below diagram illustrates the high steps involved in setting up
    authentication and authorization.

![](media/0a675fcdcc7472c343988c96f6d85583.png)

### 

### How to Configure Users and Roles on Specific Cloud IDPs

The following table highlights the various permissions supported by KM Metadata
Service, and what endpoints are authorized by each permission.

| Permission Name      | Allows endpoints                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | HTTP Method     |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| admin                | \*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                 |
| view_domain_metadata | /dkm/{dkm_id} /domains /domains/{domain_id} /term_attribute_matchers /term_attribute_matchers/{matcher_id} /catalogs /catalogs/{catalog_id} /data_sources /data_sources/{data_source_id} /terms /terms/{term_id} /fetchers /fetchers/{fetcher_id} /data_mappings data_mappings/{mapping_id}                                                                                                                                                                                                                                                  | GET             |
| view_app_metadata    | /applications /agents/{agent_id} /applications/{app_id}/schema /applications/{app_id}/schema/relationships /applications/{app_id}/schema/entities /applications/{app_id}/schema/{term_id} /applications/{app_id}/datasources /applications/{app_id}/datasources/{datasource_id} /applications/{app_id}/fetchers /applications/{app_id}/fetchers/{fetcher_id} /applications/{app_id}/term_attribute_matchers /applications/{app_id}/term_attribute_matchers/{matcher_id} /applications/{app_id}/term_attribute_matchers/{matcher_id}/artifact | GET             |
| update_app_metadata  | /applications /agents/{agent_id} /applications/{app_id}/schema /applications/{app_id}/schema/relationships /applications/{app_id}/schema/entities /applications/{app_id}/schema/{term_id} /applications/{app_id}/datasources /applications/{app_id}/datasources/{datasource_id} /applications/{app_id}/fetchers /applications/{app_id}/fetchers/{fetcher_id} /applications/{app_id}/term_attribute_matchers /applications/{app_id}/term_attribute_matchers/{matcher_id} /applications/{app_id}/term_attribute_matchers/{matcher_id}/artifact | POST/PUT/DELETE |

### Setting up BST Access Management Service

This section provides details on how to map the above user roles defined in the
IDP, to service permissions on the BST Access Management Service.

![](media/a371b5024fae0196d0e69d1a602aba04.jpg)

**Step-1: Create Roles**

Invoke Roles endpoint

| POST /roles/ |
|--------------|

| curl -X 'POST' \\  'https://editor.swagger.io/v1/roles' \\  -H 'accept: application/json' \\  -H 'Content-Type: application/json' \\  -d '{  "role_id": 0,  "name": "string",  "description": "string",  "created_on": {},  "modified_on": {},  "is_modifiable": true,  "is_removable": true,  "is_system": true,  "color": "string",  "icon": "string",  "feature_permission_groups": [  {  "association_id": 0,  "feature_permission_group": [  {  "fpg_id": 0,  "name": "string",  "description": "string",  "permission_list": [  {  "association_id": 0,  "permission": [  {  "permission_id": 0,  "sp_id": "string",  "permission": "string",  "level": "visible"  }  ]  }  ]  }  ]  }  ] }' |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

**Step-2: Create Permissions Group**

Invoke Permissions endpoint

| POST /feature_permission_groups/ |
|----------------------------------|

| curl -X 'POST' \\  'https://editor.swagger.io/v1/feature_permission_groups' \\  -H 'accept: application/json' \\  -H 'Content-Type: application/json' \\  -d '{  "fpg_id": 0,  "name": "string",  "description": "string",  "permission_list": [  {  "association_id": 0,  "permission": [  {  "permission_id": 0,  "sp_id": "string",  "permission": "string",  "level": "visible"  }  ]  }  ] }' |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

**Step-3: Assign Permission Group to Roles**

| POST /roles/{role_id}/feature_permission_groups/{feature_permission_group_id} |
|-------------------------------------------------------------------------------|

| curl -X 'POST' \\  'https://editor.swagger.io/v1/roles/1/feature_permission_groups/124121' \\  -H 'accept: application/json' \\  -d '' |
|----------------------------------------------------------------------------------------------------------------------------------------|

**Step-4: Create User Permissions**

| POST /service_providers/{sp_id}/permissions |
|---------------------------------------------|

| curl -X 'POST' \\  'https://editor.swagger.io/v1/service_providers/sp1/permissions' \\  -H 'accept: application/json' \\  -H 'Content-Type: application/json' \\  -d '[  {  "permission_id": 0,  "sp_id": "string",  "permission": "string",  "level": "visible"  } ]' |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

**Step-5: Assign Permissions to Permission Group**

| POST /feature_permission_groups/{feature_permission_group_id}/permissions/{permission_id} |
|-------------------------------------------------------------------------------------------|

| curl -X 'POST' \\  'https://editor.swagger.io/v1/feature_permission_groups/12/permissions/12314' \\  -H 'accept: application/json' \\  -d '' |
|----------------------------------------------------------------------------------------------------------------------------------------------|

## 

### Make your First Call

This section will walk you through making your first API call to get domain
information.

Format your request using the curl example below.

| curl -X 'GET' \\  'https://{km_metadata_service_server}/domains?app_id=app_id' \\  -H 'accept: application/json'  |
|-------------------------------------------------------------------------------------------------------------------|

### 

### API Request and Response

| Endpoint        | /domains                                                                                                                                                               |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Request Body    | https:// {km_metadata_service_server}/domains?app_id=12                                                                                                                |
| Response Format | [  {  "id": "string",  "type": "Domain",  "label": "string",  "description": "string",  "has_dkm": [  {  "id": "string",  "type": "DKM",  "label": "string"  }  ]  } ] |

### Abbreviations and Terms

| **Abbreviations** | **Definition**  |
|-------------------|-----------------|
| KM                | Knowledge Mesh  |
| KG                | Knowledge Graph |

# Chapter 3 API Reference

This section lists all the available API endpoints, that allow you to use the
different capabilities provided by the KM metadata services.  

Domain Knowledge Model

[Domains](#domains)

[Applications](#_Toc93136777)

### Domain Knowledge Model

Domain knowledge API allows you to get the details of a specific domain
knowledge model. Each domain knowledge model is identified by a unique id, which
is made up of a string and is unique to the that domain.

### Details of a knowledge model

| HTTP Method | GET                                       |
|-------------|-------------------------------------------|
| End Point   | /dkm/{dkm_id}                             |
| Description | Get details of a specific knowledge model |

Request Parameters

| **Name** | **Description**            | **Data Type** | **Required** |
|----------|----------------------------|---------------|--------------|
| dkm_id   | URL encoded ID of the DKM. | String        | Yes          |

Example Request

|  curl GET https://{km_metadata_service_server}/dkm/{dkm_id} |
|-------------------------------------------------------------|

Response Parameters

| **Name** | **Description**                       | **Data Type** | **Required** |
|----------|---------------------------------------|---------------|--------------|
| id       | Unique identifier for the given user. | string        | Yes          |
| type     | Type                                  | DKM           | Yes          |
| label    | Label                                 | string        | Yes          |

### 

Example Response

| {  "id": "string",  "type": "DKM",  "label": "string" } |
|---------------------------------------------------------|

### 

Response Status

| **Name** | **Description**      |
|----------|----------------------|
| 200      | successful operation |
| 401      | Unauthorized         |
| 404      | User not found       |

### Domains

The Domain API provides access to a list of available domains as well as
detailed information on a given domain.

### List of available domains

| HTTP Method | GET                               |
|-------------|-----------------------------------|
| End Point   | /domains                          |
| Description | Get the list of available domains |

Request Parameters

| **Name** | **Description**                                            | **Data Type** | **Required** |
|----------|------------------------------------------------------------|---------------|--------------|
| App_id   | URL encoded app ID by which the results are to be filtered | String        | No           |

Example Request

| curl -X 'GET' \\  'https://editor.swagger.io/domains' \\  -H 'accept: application/json' |
|-----------------------------------------------------------------------------------------|

Response Parameters

| **Name**    | **Description**                       | **Data Type** |
|-------------|---------------------------------------|---------------|
| id          | Unique identifier for the given user. | string        |
| type        | types                                 | DKM           |
| label       | Label                                 | string        |
| description | description                           | string        |
| has_dkm     | dkm                                   | string        |

Example Response

| [  {  "id": "string",  "type": "Domain",  "label": "string",  "description": "string",  "has_dkm": [  {  "id": "string",  "type": "DKM",  "label": "string"  }  ]  } ] |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

Response Status

| **Name** | **Description**      |
|----------|----------------------|
| 200      | successful operation |
| 401      | Unauthorized         |
| 404      | resource not found   |

### Details of a specific domain

| HTTP Method | GET                              |
|-------------|----------------------------------|
| End Point   | /domains/{domain_id}             |
| Description | Get details of a specific domain |

Request Parameters

| **Name**  | **Description**                         | **Data Type** | **Required** |
|-----------|-----------------------------------------|---------------|--------------|
| domain_id | Unique identifier for the given domain. | String        | Yes          |

Example Request

|  curl GET https://{km_metadata_service_server}/domains/{domain_id}  |
|---------------------------------------------------------------------|

Response Parameters

| **Name**    | **Description** | **Data Type** |
|-------------|-----------------|---------------|
| id          | Id              | string        |
| type        | Type            | dkm           |
| label       | Label           | string        |
| description | Description     | string        |
| has_dkm     | dkm             | string        |

Example Response

| [  {  "id": "string",  "type": "Domain",  "label": "string",  "description": "string",  "has_dkm": [  {  "id": "string",  "type": "DKM",  "label": "string"  }  ]  } ] |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

Response Status

| **Name** | **Description**      |
|----------|----------------------|
| 200      | successful operation |
| 401      | Unauthorized         |
| 404      | resource not found   |

### Applications

The Application API gives you access to a list of all the applications that have
been registered in the system, as well as the ability to create, update, and
delete specific applications.

### List of Registered Application in the System

| HTTP Method | GET                                               |
|-------------|---------------------------------------------------|
| End Point   | /domains                                          |
| Description | Get list of applications registered in the system |

Example Request

| curl -X 'GET' \\  'https://editor.swagger.io/applications' \\  -H 'accept: application/json' |
|----------------------------------------------------------------------------------------------|

Response Parameters

| **Name**  | **Description**                                                  | **Data Type** |
|-----------|------------------------------------------------------------------|---------------|
| id        | Unique identifier for the given application.                     | string        |
| type      | types                                                            | DKM           |
| label     | Label                                                            | string        |
| has_owner | A list of agent IDs that represent the owners of the application | string        |
| domains   | List of domains IDs applicable to this application.              | string        |

Example Response

| [  {  "type": "Application",  "id": "string",  "label": "string",  "has_owner": [  "string"  ],  "domains": [  "string"  ]  } ] |
|---------------------------------------------------------------------------------------------------------------------------------|

Response Status

| **Name** | **Description**      |
|----------|----------------------|
| 200      | successful operation |
| 401      | Unauthorized         |

