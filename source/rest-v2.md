---
title: API Reference

language_tabs:
  - shell

toc_footers:
 - <a href='https://publitas.com'>Publitas.com</a>
 - <a href='http://github.com/tripit/slate'>Powered by Slate</a>
---

# REST API v2

This API can be used to programatically list your account's groups, as well as to list and create publications based on existing PDF documents.

### Version

Our API is currently on version 2 beta. Click [here](index.html) to see documentation for the first version of the API.

### Authentication

Our public API requires an API key, sent with the parameter `api_key`. Contact our [support team](mailto:support@publitas.com) to request an API key. For convenience, we also allow HTTP based authorization with the `api` user and your API key as password.

### Formats

Currently we only support JSON requests.

### Paths

The base path of an API request is: `https://api.publitas.com/v2/`

An example of a complete path is:

`https://api.publitas.com/v2/groups/?api_key=<api_key>`

# Groups

```shell
# This will retrieve all the groups accessible by the user
curl "https://api.publitas.com/v2/groups?api_key=<api_key>"
```
> The above command returns a JSON document structured like so:

```json
{
  "groups": [
    {
      "id": 1,
      "title": "Example group",
      "slug": "example-group",
      "url": "https://api.publitas.com/v2/groups/1",
      "publications_url": "https://api.publitas.com/v2/groups/1/publications",
      "publication_count": 2,
      "public_url": "https://view.publitas.com/example-group"
    }
  ]
}
```

The attributes are as follows:

|       Field       |   Type  |                    Description                             |
|-------------------|---------|------------------------------------------------------------|
| id                | Integer | Group ID                                                   |
| title             | String  | Group title                                                |
| slug              | String  | Group slug                                                 |
| url               | String  | Group details URL                                          |
| publications_url  | String  | Publication list URL for the group                         |
| publication_count | Integer | Amount of publications contained within the group          |
| public_url        | String  | Group public URL (redirects to latest online publication)  |

# Publications

## List all publications of a group

```shell
# This will retrieve all publications for a group
curl "https://api.publitas.com/v2/groups/1/publications?api_key=<api_key>"
```
> The above command returns JSON structured like this:

``` json
{
  "publications": [
    {
      "id": 1,
      "title": "Spring 2014",
      "slug": "spring-2014",
      "url": "https://api.publitas.com/v2/groups/1/publications/1",
      "cover_url": "https://view.publitas.com/1/1/pages/fb7be8c8211c15f3b5aa6e7527fe6fe2efb4f60c-at800.jpg",
      "page_count": 52,
      "state": "offline",
      "online_at": null,
      "offline_at": null,
      "schedule_online_at": null,
      "schedule_offline_at": null,
      "public_url": "https://view.publitas.com/example-group/spring-2014"
    },
    {
      "id": 2,
      "title": "Autumn 2014",
      "slug": "autumn-2014",
      "url": "https://api.publitas.com/v2/groups/1/publications/2",
      "cover_url": "https://view.publitas.com/1/2/pages/fb7be8c8211c15f3b5aa6e7527fe6fe2efb4f60c-at800.jpg",
      "page_count": 52,
      "state": "offline",
      "online_at": null,
      "offline_at": null,
      "schedule_online_at": "2014-09-25T15:17:20.000+02:00",
      "schedule_offline_at": null,
      "public_url": "https://view.publitas.com/example-group/autumn-2014"
    }
  ]
}
```

### HTTP Request

`GET https://api.publitas.com/v2/groups/<Group ID>/publications`

### URL Parameters

Parameter | Description
--------- | -----------
Group ID | The ID of a specific group


The JSON response returns a list of publications with the following attributes:

|        Field        |   Type   |                           Description                                         |
|---------------------|----------|-------------------------------------------------------------------------------|
| id                  | Integer  | Publication ID                                                                |
| title               | String   | Publication Title                                                             |
| slug                | String   | Publication Slug                                                              |
| url                 | String   | Publication details URL                                                       |
| cover_url           | String   | URL for the cover image (@800 resolution)                                     |
| page_count          | Integer  | Number of pages in the publication                                            |
| state               | String   | The publication state (see table below for a better description)              |
| online_at           | DateTime | Time at which the publication was last set online                             |
| offline_at          | DateTime | Time at which the publication was last set offline                            |
| schedule_online_at  | DateTime | Time at which the publication is scheduled to go online                       |
| schedule_offline_at | DateTime | Time at which the publication is scheduled to go offline                      |
| public_url          | String   | Publication public URL when state is `online` or `offline`. Otherwise, `null` |

The `state` field can have one of the following values:

|    Value    |                        Description                     | Visible on CMS |
|-------------|--------------------------------------------------------|----------------|
| new         | Initial state of a publication.                        | No             |
| preparing   | Preparing PDF for conversion.                          | Yes            |
| converting  | PDF is being converted and awaiting result.            | Yes            |
| failed      | Usually triggered with invalid PDFs.                   | Yes            |
| offline     | Publication is not publicly visible.                   | Yes            |
| online      | Publication is publicly visible.                       | Yes            |

## Get a specific publication

```shell
# This endpoint retrieves a specific publication.
curl "https://api.publitas.com/v2/groups/1/publications/222?api_key=<api_key>"
```
> The above command returns JSON structured like this:

``` json
{
  "publication": [
    {
      "id": 222,
      "title": "Spring 2014",
      "slug": "spring-2014",
      "url": "https://api.publitas.com/v2/groups/1/publications/222",
      "cover_url": "https://view.publitas.com/1/222/pages/fb7be8c8211c15f3b5aa6e7527fe6fe2efb4f60c-at800.jpg",
      "page_count": 52,
      "state": "offline",
      "online_at": null,
      "offline_at": null,
      "schedule_online_at": null,
      "schedule_offline_at": null,
      "public_url": "https://view.publitas.com/example-group/spring-2014"
    }
  ]
}
```

### HTTP Request

`GET https://api.publitas.com/v2/groups/<Group ID>/publications/<Publication ID>`

### URL Parameters

Parameter | Description
--------- | -----------
Group ID | The ID of a specific group
Publication ID | The ID of a specific publication


## Create a publication

```shell
# This will create a publication with the title Winter2014, and source URL http://example.com/winter2014.pdf.
curl --data "publication[title]=Winter2014&publication[source_url]=http://example.com/winter2014.pdf" "https://api.publitas.com/v2/groups/1/publications?api_key=<api_key>"
```
> The above command returns JSON structured like this:

```json
{
  "publication": {
    "id": 3,
    "title": "Winter2014",
    "slug": "winter2014",
    "url": "https://api.publitas.com/v2/groups/1/publications/3",
    "cover_url": null,
    "page_count": 0,
    "state": "preparing",
    "online_at": null,
    "offline_at": null,
    "schedule_online_at": null,
    "schedule_offline_at": null,
    "public_url": null
  }
}
```

### HTTP Request

`POST https://api.publitas.com/v2/groups/<Group ID>/publications`

### URL Parameters

Parameter | Description
--------- | -----------
Group ID | The ID of a specific group


### Request body parameters

The following fields need to be sent within a publication scope (see right for an example.)

|         Name        |   Type   | Required |                                                                               Description                                                                |
|---------------------|----------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| source_url          | String   | Yes      | URL where the PDF file resides. HTTP and HTTPS are accepted, and it needs to be a public accessible file.                                                |
| title               | String   | Yes      | The title for the publication                                                                                                                            |
| language            | String   | No       | 2-digit language code. See [the language table](#languages) below for allowed values                                                                     |
| schedule_online_at  | DateTime | No       | Time at which the publication is scheduled to be online. If the current time is provided, the publication will be put online as soon as it is converted  |
| schedule_offline_at | DateTime | No       | Time at which the publication is scheduled to be offline                                                                                                 |

When created, a publication is automatically queued to be converted. The conversion status can be monitored by requesting the URL that you find in the `url` key.

For a publication created via the API, the state path should be:

`new` > `preparing` > `converting` > `offline`

## Mark a publication as online

```shell
# This endpoint marks a publication as being online.
curl -X POST "https://api.publitas.com/v2/groups/1/publications/222/online?api_key=<api_key>"
```

> When the response code is 200 the command returns JSON structured like this:

``` json
{
  "publication": [
    {
      "id": 222,
      "title": "Spring 2014",
      "slug": "spring-2014",
      "url": "https://api.publitas.com/v2/groups/1/publications/222",
      "cover_url": "https://view.publitas.com/1/222/pages/fb7be8c8211c15f3b5aa6e7527fe6fe2efb4f60c-at800.jpg",
      "page_count": 52,
      "state": "online",
      "online_at": "2015-01-28T16:05:28.309+01:00",
      "offline_at": null,
      "schedule_online_at": null,
      "schedule_offline_at": null,
      "public_url": "https://view.publitas.com/example-group/spring-2014"
    }
  ]
}
```

### HTTP Request

`POST https://api.publitas.com/v2/groups/<Group ID>/publications/<Publication ID>/online`

### Response codes

This endpoint returns one of the following HTTP codes:

| Code | Description                             |
|------|---------------------------------------- |
| 200  | Publication is online                   |
| 402  | You have reached your publishing limit. |

## Mark a publication as offline

```shell
# This endpoint marks a publication as being offline.
curl -X POST "https://api.publitas.com/v2/groups/1/publications/222/offline?api_key=<api_key>"
```

> The above command returns JSON structured like this:

``` json
{
  "publication": [
    {
      "id": 222,
      "title": "Spring 2014",
      "slug": "spring-2014",
      "url": "https://api.publitas.com/v2/groups/1/publications/222",
      "cover_url": "https://view.publitas.com/1/222/pages/fb7be8c8211c15f3b5aa6e7527fe6fe2efb4f60c-at800.jpg",
      "page_count": 52,
      "state": "offline",
      "online_at": "2015-01-28T16:05:28.309+01:00",
      "offline_at": "2015-01-28T17:05:28.309+01:00",
      "schedule_online_at": null,
      "schedule_offline_at": null,
      "public_url": "https://view.publitas.com/example-group/spring-2014"
    }
  ]
}
```
### HTTP Request

`POST https://api.publitas.com/v2/groups/<Group ID>/publications/<Publication ID>/offline`

### Response codes

This endpoint returns the `200` response code.

# Languages

|    Name    | Code |
|------------|------|
| Basque     | eu   |
| Bulgarian  | bg   |
| Chinese    | zh   |
| Croatian   | hr   |
| Czech      | cs   |
| Danish     | da   |
| Dutch      | nl   |
| English    | en   |
| French     | fr   |
| Finnish    | fi   |
| German     | de   |
| Hungarian  | hu   |
| Italian    | it   |
| Lithuanian | lt   |
| Norwegian  | no   |
| Polish     | pl   |
| Portuguese | pt   |
| Romanian   | ro   |
| Russian    | ru   |
| Serbian    | sr   |
| Slovak     | sk   |
| Spanish    | es   |
| Swedish    | sv   |
| Turkish    | tr   |
| Ukranian   | uk   |

# Errors

> When there is an error the command returns JSON structured like this:

``` json
{
  "errors": {
    "state": [
        "You have reached your publication limit"
    ]
  }
}
```

The Publitas API uses the following error codes:

| Error Code |                                          Meaning                                                                                     |
|------------|--------------------------------------------------------------------------------------------------------------------------------------|
|        402 | Payment Required -- You have reached your publishing limit                                                                           |
|        404 | Not Found -- The path or document could not be found                                                                                 |
|        405 | Method Not Allowed -- You tried to access the API with an invalid method                                                             |
|        406 | Not Acceptable -- You requested a format that isn't json                                                                             |
|        418 | I'm a teapot                                                                                                                         |
|        422 | Unprocessable Entity -- We found validation errors on one or more fields, a detailed error message can be found in the response body |
|        500 | Internal Server Error -- We had a problem with our server. Try again later                                                           |
|        503 | Service Unavailable -- We're temporarially offline for maintenance. Please try again later                                           |