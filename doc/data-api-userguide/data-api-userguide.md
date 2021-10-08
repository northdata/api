![image alt text](image_0.png)

# Data API User Guide

The data API allows you to directly access North Data's company database in JSON or XML format. 

Please see also:

- **Reference guide**: https://northdata.github.io/doc/api/
- **Swagger / OpenAPI 2.0 definition file** https://github.com/northdata/api/blob/master/swagger.yaml
- **OpenAPI 3.0 definition file** https://northdata.github.io/doc/api/openapi.yaml

## Content

- [Data API User Guide](#data-api-user-guide)
  - [Content](#content)
  - [Quick start](#quick-start)
    - [Example](#example)
    - [Response format](#response-format)
    - [Authentication](#authentication)
      - [Method 1: https header](#method-1-https-header)
      - [Method 2: request parameter](#method-2-request-parameter)
    - [Error handling](#error-handling)
    - [Language support](#language-support)
    - [Country support](#country-support)
    - [Privacy protection](#privacy-protection)
  - [Retrieving single companies](#retrieving-single-companies)
    - [Identifying a company by name and city](#identifying-a-company-by-name-and-city)
    - [Identifying a company by register ID and court city](#identifying-a-company-by-register-id-and-court-city)
    - [Identifying a company by internal company ID](#identifying-a-company-by-internal-company-id)
  - [Accessing company detail information](#accessing-company-detail-information)
    - [Events](#events)
    - [Segment codes](#segment-codes)
    - [Extras provided by third parties](#extras-provided-by-third-parties)
  - [Retrieving persons](#retrieving-persons)
    - [Identifying a person by name, city and birth date](#identifying-a-person-by-name-city-and-birth-date)
    - [Identifying a person by internal person ID](#identifying-a-person-by-internal-person-id)
  - [Selecting publications](#selecting-publications)
    - [Publication structure](#publication-structure)
  - [Searching](#searching)
    - [Retrieving large result lists](#retrieving-large-result-lists)
    - [Power Search](#power-search)
      - [The `keywords` parameter](#the-keywords-parameter)
      - [Filtering by segment codes](#filtering-by-segment-codes)
      - [Filtering by financials and other performance indicators](#filtering-by-financials-and-other-performance-indicators)
      - [Filtering by events](#filtering-by-events)
    - [Universal Search](#universal-search)
    - [Auto-complete suggestions](#auto-complete-suggestions)
  - [Appendix A: Database synchronization](#appendix-a-database-synchronization)
  - [Appendix B: Company entry merger scenarios](#appendix-b-company-entry-merger-scenarios)
  - [Appendix C: Company lifecycle and event types](#appendix-c-company-lifecycle-and-event-types)
  - [Appendix D: Publication sources](#appendix-d-publication-sources)
  - [Appendix E: Topic types](#appendix-e-topic-types)
  - [Appendix F: Role types](#appendix-f-role-types)
  - [Appendix G: Event types](#appendix-g-event-types)

## Quick start

An API key is required for using the Data API. To obtain an API key, please contact support@northdata.com.

### Example

The request:

https://www.northdata.com/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&api_key=XXXX_XXXX

will result in:

```json
{
  "id": "57514825",
  "name": {
    "name": "1000mikes AG",
    "legalForm": "AG"
  },
  "address": {
    "street": "Hansaplatz 4",
    "postalCode": "20099",
    "city": "Hamburg",
    "country": "DE",
    "lat": 53.5541139,
    "lng": 10.0116615
  },
  "register": {
    "city": "Hamburg",
    "id": "HRB 103038",
    "uniqueKey": "12203550103038"
  },
  "status": "liquidation"
}
```
If you use *get* requests, please ***make sure to properly encode parameters***. 

### Response format

The default response format is JSON. To enable XML responses add the parameter 
`output=xml`. The request:

https://www.northdata.com/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&api_key=XXXX_XXXX&output=xml

will result in:

```xml
<Company>
  <id>57514825</id>
  <name>
    <name>1000mikes AG</name>
    <legalForm>AG</legalForm>
  </name>
  <address>
    <street>Hansaplatz 4</street>
    <postalCode>20099</postalCode>
    <city>Hamburg</city>
    <country>DE</country>
    <lat>53.5541139</lat>
    <lng>10.0116615</lng>
  </address>
  <register>
    <city>Hamburg</city>
    <id>HRB 103038</id>
    <uniqueKey>12203550103038</uniqueKey>
  </register>
  <status>LIQUIDATION</status>
</Company>
```

### Authentication

The API is accessed via secure **https** *get* or *post* requests. An API key of the form *XXXX-XXXX* is required.
There are two methods to pass the API key. For production purposes, method 1 with *post* requests is recommended.  

#### Method 1: https header

Add the API key as an http header:

```
X-Api-Key: XXXX-XXXX
```

#### Method 2: request parameter

Add the API key as a *post* or URL parameter `api_key` such as in:

https://www.northdata.com/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&api_key=XXXX_XXXX

### Error handling

HTTP status codes are used to provide error information.

Status code | Explanation
---|----
200 OK | 
400 Bad Request | Invalid parameters
404 Not found | Company / Person / Publication not found
500 Internal Error | Please contact our support team
503 Service Unavailable | Please retry the request (e.g., up to 3 times)

### Language support

The URL parameter *language* may be used to select a supported language for all "textual" content, such as role designations and event descriptions. 

Language code | Explanation
---|----
de | German (default)
en | English 

### Country support

This API may be used to access all supported countries.
We continuously extend our country support and add new sources. The current support is documented in our online reference page: 

https://www.northdata.com/_coverage

This page also covers performance indicators and their availability.


### Privacy protection

Company data may include personal data, i.e., data about the legal representatives of the 
company. As such, it may be subject to take down requests according to the 
EU "right to be forgotten" or other privacy protection law. 

Thus, if you intend to display data on a public web page, i.e., on a web page that does not require
user registration and/or allows access to bots such as the Googlebot, then you are required to 
use the following mechanisms for privacy protection. 

 1. Append the parameter `censor=true` to all data API calls where the response data will be display on a public web page. 
 2. Company, person and publication data responses have a boolean field `blocked`. If this field is set to true, the response data should not be used on a public page.

These mechanisms make sure that data where take-down request have been filed or data 
that is generally considered as to be too sensitive for public display (e.g., personal 
insolvency filings) is handled properly. 

## Retrieving single companies

The following requests are available to query a single company:

Request | URL
------- | ---------
retrieve company | https://www.northdata.com/_api/company/v1/company
retrieve publications of a company  | https://www.northdata.com/_api/company/v1/publications

The company is referenced using one or more of the following parameters. Which combinations to use depends on your use case and will be explained further below. 

Parameter name | Type | Explanation
---------------|------|------------
`name` | string | full company name (including legal form)
`address` | string | company address (you only need to specify the city)
`registerId` | string | the ID of under which the company is registered, for example HRB 12345
`registerCity` | string | the city of the court where the company is registered, for example Hamburg
`registerKey` | string | key that has been provided via the *register.uniqueKey* field 
`companyId` | string | internal company id (do not store this id in an external database, it may change over time)
`fuzzyMatch` | boolean | true to find best match (similar name and nearby address) 

These parameters may be used in the following combinations (Germany only). If you pass more parameters than required, the system will try all of them.

### Identifying a company by name and city

A company may be identified using the combination of name and city:

```
name=1000MIKES AG
address=Hamburg
```

If you use these parameters in a URL request make sure to encode the parameters properly, e.g:

```
name=1000MIKES%20AG&address=Hamburg
```

Street name and zip code are not required. In Germany, company name and city uniquely identify a company (at a given point of time, which is the time of the request). 

Common differences in writing such as uppercase/lowercase, German umlauts and abbrevations will be handled properly. All of the following are equivalent:

```
name=1000MIKES AG
name=1000mikes AG
name=1000mikes Aktiengesellschaft
```

Also different spellings of the city name will be handled properly, and the following are equivalent:

```
address=Frankfurt
address=Frankfurt am Main
address=Frankfurt a. Main
```

Still, different spellings may cause headaches. If you're not sure whether your name and address data is correct, there is another parameter that may help you:

```
fuzzyMatch=true
```

If this parameter is set, the best matching company is chosen using reasonable probability tresholds. 

### Identifying a company by register ID and court city

German companies that are registered with the German Handelsregister may be identified with their Handelsregister ID and the court city (where the register is located). The combination uniquely identifies a company:

```
registerId=HRB 103038
registerCity=Hamburg
```

Different spellings will be handled propertly, e.g.,

```
registerCity=Ludwigshafen
registerCity=Ludwigshafen a. Rh. 
```

are equivalent. Also, changes caused by the fusion of courts are handled properly. For example, the entry:

```
registerId=HRB 111
registerCity=Husum
```

is equivalent to this one (after the *Husum* register was taken over by *Flensburg*)

```
registerId=HRB 111 HU
registerCity=Flensburg
```

To make it easier for you to reference a company, we encode the register identifier into a  unique key so that you don't have to keep track of ID and city. So instead of using `registerId` and `registerCity`, a single parameter may be used:

```
registerKey=12203550103038
```

The unique key can be found in the `register.uniqueKey` field of the company data structure. If you plan to store company data in your database and you want to refer to the North Data database, you would store this unique key (instead of the internal North Data company ID, because the internal company ID might change over time). More information regarding database synchronization and usage of the unique key is available in the appendix.

### Identifying a company by internal company ID

Companies might also be identified using a `companyId`. This is strongly discouraged, because there are various scenarios where this ID may change overtime (which sounds strange, but there are reasons, as explained in  [Appendix B]((#appendix-b-company-entry-merger-scenarios))). Please use the unique register key instead (see previous section).

## Accessing company detail information

All of the requests that return information for one or multiple companies:

Request | URL
------- | ---------
retrieve company | https://www.northdata.com/_api/company/v1/company
power search | https://www.northdata.com/_api/search/v1/power
universal search | https://www.northdata.com/_api/search/v1/universal

will return only base data by default. 

If you want more detail, you may add one or more of the following parameters.

Parameter name | Type | Explanation
---------------|------|------------
`history` | boolean | true to include historical data
`financials` | boolean | true to include financial performance indicators
`mktgtech` | boolean | true to include mktg & tech performance indicators
`sheets` | boolean | true to include sheets (balance sheet, earnings)
`events` | boolean | true to include event data 
`eventTypes` | boolean | restrict which event types will be returned if `events` equals true 
`maxEvents` | number | maximum number of events to return 
`relations` | boolean | true to include related company and person data
`extras` | boolean | true to include detail company data provided by 3rd parties

If `history` is set to true, the `name`, `address` and `register` history is added to the API response. If `history` is set to true in combination with `financials`, then the known financial history is added to the response.  If `history` is set to true in combination with `relations`, then also formerly related companies and persons are included.

### Events

Events of a company are changes in the company lifecycle, changes of base data such as name, address, legal form or base capital, or changes in management. For a complete list, see [Appendix C](#appendix-c-company-lifecycle-and-event-types).

If the `events` parameter is set to true, all events for a company will be returned. These may be many, and it is recommended to restrict the response size by specifying the `maxEvents` parameter and the `eventTypes` parameter. For example:

```
events=true
eventTypes=NameChange|AddressChange
maxEvents=3
```

and the resulting request URL is:

https://www.northdata.com/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&events=true&eventTypes=NameChange|AddressChange&maxEvents=3&api_key=XXXX_XXXX

### Segment codes

The segment codes (industry classification) of a company are reported in the field `segmentCodes`. These codes are provided for various common international standards, e.g.:

```
segmentCodes: {
  "wz" : [ "01.13.2" ],
  "uksic" : [ "01.13" ],
  "isic" : [ "0113" ],
  "nace" : [ "01.13" ],
  "naics" : [ "111991", "111211", "111411", "111219", "111419" ]
}
```
For each standard, a list of segment codes is given. The property name will always be the name of the standard in lowercase. Supported
standards are:


Standard name | Revision | Note
--|--|--
ISIC | 4.0 | [International Standard Industrial Classification (United Nations industry classification system)](https://unstats.un.org/unsd/classifications/)
NAICS | 2017 | [North American Industry Classification System (classification of business establishments used in Canada, the US and Mexico)](https://www.census.gov/eos/www/naics/)
NACE | Rev. 2 | [Statistical Classification of Economic Activities in the EU](https://ec.europa.eu/eurostat/web/nace-rev2)
WZ | 2008 | [German classification system based on the EU NACE Standard](https://www.destatis.de/DE/Methoden/Klassifikationen/Gueter-Wirtschaftsklassifikationen/klassifikation-wz-2008.html)
UKSIC | 2007 | [UK standard industrial classification of economic activities](https://www.ons.gov.uk/methodology/classificationsandstandards/ukstandardindustrialclassificationofeconomicactivities/uksic2007)


### Extras provided by third parties

*Extras* are data items that cannot be determined from official company filings. We obtain them from third party data providers that evaluate sources such as company websites. Set the `extras` parameter to true in order to retrieve them,  

Extra Id | Explanation
---|---
phone | Company phone number 
fax | Fax number 
email | Email address
url | Website URL
vatId | VAT (value-added text) ID

## Retrieving persons

The following request is available to query a single person:

Request | URL
------- | ---------
retrieve person | https://www.northdata.com/_api/person/v1/person
retrieve publications associated with a person | https://www.northdata.com/_api/person/v1/publications

The person is referenced using one or more of the following parameters (which combinations to use depends on your use case and will be explained below).

Parameter name | Type | Explanation
---------------|------|------------
`firstName` | string | first name
`lastName` | string | last name
`address` | string | person address (you only need to specify the city)
`birthDate` | date | birth date
`personId` | string | internal company id (do not store this id in an external database, it may change over time)

These parameters may be used in the following combinations (Germany only).

### Identifying a person by name, city and birth date

The following combination might be used:
```
firstName=Alfons
lastName=Schuhbeck
address=München
birthDate=1949-05-02
```

A properly encoded *get* request might look like:

https://www.northdata.com/_api/person/v1/person?firstName=Alfons&lastName=Schuhbeck&address=M%C3%BCnchen&birthDate=1949-05-02

### Identifying a person by internal person ID

Persons might also be identified using a `personId`. This is strongly discouraged, because there are various scenarios where this ID may change overtime. Please contact us for discussion if you need more detail on this.

## Selecting publications

The followinging request are available to retrieve publications:

Request | URL
------- | ---------
query publications | https://www.northdata.com/_api/pub/v1/publications
retrieve publications associated with a company | https://www.northdata.com/_api/company/v1/publications
retrieve publications associated with a person | https://www.northdata.com/_api/person/v1/publications

These requests share several parameters:

Parameter name | Type | Explanation
---------------|------|------------
`source` | string | sources of the returned publications 
`content` | boolean | whether to include publication text / html 

For a list of possible source ids see [Appendix D](#appendix-d-publication-sources).

The default for the `source` parameter is `source=Hrb`. For a list of possible source ids see [Appendix D](#appendix-d-publication-sources).

### Publication structure

The result of our analysis of a particular publication is called the "structure" of a publication, which is simply a list of "topics". Each
topic has a type, a language-dependant, human readable name and an optional value. A list of all topic types can be found in 
[Appendix E](#appendix-e-topic-types).


## Searching 

The following requests are available for searching:

Request | URL
------- | ---------
power search | https://www.northdata.com/_api/search/v1/power
universal search | https://www.northdata.com/_api/search/v1/universal
suggest (auto complete) | https://www.northdata.com/_api/search/v1/suggest

*Power search* is the most flexible search method. It allows you to specify many different criteria, such as regional, financial, and more.

*Universal Search* provides best matches for a single query string. As such, it's major use case is the implementation of search boxes.

*Suggest* is specifically optimized for implementing auto complete
in input boxes (as you type). 

### Retrieving large result lists

A search might have many results that do not fit into a simple http response (the default 
limit is 15 results). In order to retrieve many / all responses, please use the following 
position mechanism. 

If not all results fitted in the response, a field `nextPos` is set in the response. Take the value of this field, and append it to the request as an additional parameter `pos=XXX`. 
Repeat this as long as the field `nextPos` is not empty.

### Power Search

*Power search* allows you to search companies matching many different criteria.

All lists are pipe-separated (i.e., with the '|' character).

Parameter name | Type | Explanation
---------------|------|------------
`keywords` | string | keywords to match in the company name, subject or segment
`address` | string | address (any level of precision, from house to country)
`maxDistanceKm` | number | maximum distance from given address 
`status` | string array | list of valid statuses (active, terminated, liquidation)
`countries` | string array | list of countries to include (two letter ISO codes)
`segmentCodes` | string array | list of segment codes to match
`segmentCodeStandard` | string | the segment code standard to use 
`indicatorId`  | string array | list of indicator ids for performance filtering (see [Performance indicators reference](https://www.northdata.com/_financials))
`lowerBound` | number array | list of lower bounds for performance filterings
`upperBound` | number array | list of upper bounds for performance filterings
`lowerBoundUnit` | string array | list of lower bound units (by default, this is 'EUR')
`upperBoundUnit` | string array | list of upper bound units (by default, this is 'EUR')
`eventType`  | string array | list of event types for event filtering (see [Appendix C](#appendix-c-company-lifecycle-and-event-types))
`minDate`  | date array | list of minimum dates for event filtering
`maxDate`  | date array | list of maximum dates for event filtering
`keepAlive`  | boolean | set this to false if you don't need the `nextPos` value in the result

_** Throttling: **_ Because of limitations of the underlying database (Elasticsearch), you are only allowed 
to submit one concurrent power search request. Parallel requests will not fail, 
but the position mechanism as explained above will not work any longer, because the position is cleared
by every new request.

Please note that all the [parameters for accessing company detail information](#accessing-company-detail-information) may be used. 

Please see our help center article: [Power Search: Overview](https://help.northdata.com/en/center/power-search-overview)

#### The `keywords` parameter 

The keywords parameter may include or one or more keywords to match in the company name, segment or subject.

```
keywords=restaurant cafe lunch
```
Please also see our help center article: [Combining Keywords in the Power Search](https://help.northdata.com/en/center/keywords)

#### Filtering by segment codes

This example shows how to do segment filtering. 

```
segmentCodeStandard=WZ
segmentCodes=01.11|01.12
```

TBD.: reference list of segment code standards.

Please also see our help center article: [Industry segment classification](https://help.northdata.com/en/center/industry-segment-classification)


#### Filtering by financials and other performance indicators

Multiple ranges may be specified. For example, to filter for companies that had earnings in the range of €1 - €2M and 50 - 100 employees, use the following parameters:

```
indicatorId=Earnings|Employees
lowerBound=1000000|50
upperBound=2000000|100
```
A reference list of performance indicators is provided [here](https://www.northdata.com/_financials).
For open (unlimited) ranges just omit the number. 
For the filtering, the company's last known performance indicators are used for filtering. For financial indicators this is
usually the year of the last yearly report, for marketing & tech filters simply the last year.

The search results will match all filters (although some programming languages use the '|' character for *"or"* operations, here it is a simple separator and the the filters have *"and"* semantics.)

Foreign currencies will automatically be converted to EUR.

Please also see our help center article: [Power Search: Performance Indicators](https://help.northdata.com/en/center/power-search-performance-indicator)

#### Filtering by events

The power search allows to filter result for companies where particular events happened in a particular date period. 

For example, in order to filter for companies that have been founded in 2018 and filed for insolvency in 2019, the following parameters may be used.

```
eventType=NewCompany|Insolvency
minDate=2018-01-01|2019-01-01
maxDate=2019-01-01|
```
For open (unlimited) ranges simply use an empty date.

Only the last recent events of a particular type are considered. For example, if you specify `ManagementChange` as the event type, only companies with the **last** management change in the given time period are returned, not companies with **any** management change.

Please also see our help center article: [Power Search: Event Filter](https://help.northdata.com/en/center/event-filter)

### Universal Search

The universal search tries to provide a best interpretation for the given search string. 

Parameter name | Type | Explanation
---------------|------|------------
`query` | string | the search string
`domain` | string | "company", "person" or empty to match any
`status` | string array | list of valid statuses (active, terminated, liquidation)
`countries` | string array | list of countries to include (two letter ISO codes)

The following table gives some examples of how the interpretation works.

Query | Interpretation
--|--
Anything | Name of company or person
XXX GmbH | Company 
Frank Miller | Person
Miller, Frank | Person
Anything, Hamburg | Company or person in a given city
Amtsgericht Hamburg, HRB 2000 | Register ID

Please note that all the [parameters for accessing company detail information](#accessing-company-detail-information) may be used. 

### Auto-complete suggestions

This is a very fast search request providing only base data on the search results. It has been designed for usage with auto-complete input boxes (*search-as-you-type*). It returns search results where the current or former name of the result starts with the given search string. 

This request is **not billed**, i.e., companies returned by this call will not be counted towards your monthly company quota.

Parameter name | Type | Explanation
---------------|------|------------
`query` | string | the search string
`domain` | string | "company", "person" or empty to match both
`status` | string array | list of valid company statuses (active, terminated, liquidation)
`countries` | string array | list of countries to include (two letter ISO codes)
`history` | boolean | true to include former company names
`censor` | boolean | please set to true when using suggestions in a public form (strongly recommended!)
`limit` | number | maximum number of results to be return

The recommended parameters for the typical usage scenario of prefilling a company input form are:

```
domain=company
status=active|liquidation
countries=DE|CH
history=false
censor=true
```

Please note that parameters for accessing company detail information can **not** be used with this request.

Please also see our help center article: [Using the Quick Search](https://help.northdata.com/en/center/using-the-quick-search)


## Appendix A: Database synchronization

This appendix describes how external databases may be kept in sync using regular updates after an export. 

Please use the publications method of the Data API (link to the reference guide)
https://northdata.github.io/doc/api/#pubV1PublicationsGet

Example (please use your API key and adjust the dates):

https://www.northdata.com/_api/pub/v1/publications?minTimestamp=2017-03-14&maxTimestamp=2017-03-15&source=Hrb&apiKey=XXXX-XXXX

This method provides all publications of a particular day and source. We recommend to run a job every day in the early morning to fetch the publications of the previous day. Each publication represents one or multiple events that may result in changes of company data. The response of the API request provides an array of publications. For each publication there is a field publisher, which contains the updated company data. 

You need to decided which publication sources are relevant to you, i.e., which sources trigger updates that are important for you to receive.
For a list of possible sources see [Appendix D](#appendix-d-publication-sources). For example, for German companies, you would select the most important sources `Hrb`, `Eb` and `Ins`, which would result in three update routines:

https://www.northdata.com/_api/pub/v1/publications?minTimestamp=2017-03-14&maxTimestamp=2017-03-15&source=Hrb&apiKey=XXXX-XXXX
https://www.northdata.com/_api/pub/v1/publications?minTimestamp=2017-03-14&maxTimestamp=2017-03-15&source=Eb&apiKey=XXXX-XXXX
https://www.northdata.com/_api/pub/v1/publications?minTimestamp=2017-03-14&maxTimestamp=2017-03-15&source=Ins&apiKey=XXXX-XXXX

In addition, you may want to set the parameters `publisherFinancials`, `publisherSheets`, `publisherRelations`, `publisherHistory`, and/or `publisherEvents` to true in order to retrieve company detail information. 

Please note that the daily number of publications is too high to fit in a single HTTPS call. (For Germany, expect up to 2.000 HR publications, 2.000 Bundesanzeiger publications, 5.000 insolvency publications a day.) Therefore, you should use a loop to fetch all the publications:

1. Set the parameters `minTimestamp` and `maxTimestamp` for the requested time period (recommendation: `minTimestamp` to the previous day and `maxTimestamp` current day. If you omit the time, 0:00 is assumed)
1. Invoke HTTPS API method
1. In case of an HTTP 503 status (service unavailable) retry the call up to two or three times
For all publications the JSON response has a field publisher with the updated company data  -> write this back, but see also below
1. Read the `newPos` field:
   *  not empty -> append as parameter `pos` to the request, continue with step 2
   *  empty -> complete!
1. Looking up company data in your database

It is important **not** to rely on the internal IDs that we provide via the API and the export. The IDs are only valid temporarily and may change over time. There are various real world cases (described in the following appendix) that require us to merge or split company records resulting in changes of the ID. 

Instead, we provide a field `uniqueKey` in `company.register` which may serve to safely identify a company. Every company has a current register and a list of historical registers. The best method to retrieve companies is to keep a table that maps registers to companies.

HR_UNIQUE_KEY | YOUR_INTERNAL_COMPANY_ID
--------------|----------------------
21478278742 | company1
89849894894 | company2
38738738733 | company2
 
Please note that you will infrequently encounter company data that has no register or unique key at all. This happens, and unfortunately there is no way to uniquely identify these companies. For example, there may be “Restaurant Shanghai, Berlin” filing insolvency. There is no way to safely match it. it In this case, you may either (1) drop the update, (2) just append it to your database, (3) solve the case manually.  

## Appendix B: Company entry merger scenarios

Sometimes, North Data needs to merge (or split) company entries. The consequence is that internal company IDs may change over time. ***You do not need to know or worry about why this is the case.*** But, we are frequently are asked for the reasons, so here they are.

The reasons are not technical, instead they are related to the federal structure of the German Handelsregister system, and to the history of the publication of mandatory company filings in Germany.

Here are some of the *background* problems of this system:

* every local Handelsregister (HR) has its own numbering scheme (with the exception of Baden-Württemberg, where it is state-wide)
* if a company moves from one HR jurisdiction to another, it will be assigned a new HR number
* there are other circumstances where a company may be assigned a new HR number (e.g., change of legal form) 
* local Handelsregister courts have been merged in the past (and will probably be merged in the future). Some of these merges haven't been handled well, and resulted in inconsistent numbering (which is history, but, as a bad consequence of this, there is no definitive list of HR number <-> company available)
* some companies (very few) are registered with more than one Handelsregister at the same time, thus, carrying two HR numbers at the same time. 
* the Bundesanzeiger, which also publishes company information (e.g., yearly reports) does not reference HR IDs 
* the combination of "company name + city" is (by law) unique for a point of time, but (for example) the Vodafone GmbH Düsseldorf in 2018 may be a different company from the Vodafone GmbH Düsseldorf in 2015. 
* some companies are not registered with the HR (you are only required to do for particular legal forms, e.g. GmbH, but not GbR). The result is messy, especially if a company changes its legal form from a HR-mandatory one to a non-mandatory one or vice versa.

We do our best to take care of handling these cases. Unfortunately, the handling results in edge cases where we need to merge company entries, resulting in ID changes. Some of these cases are:

* Company moves into the area of different Handelsregister court. The new HR is quick in announcing the new entry, and does not point out that this was a move from another HR (some do, but some do not). Thus, we allocate a new company entry. Later, the old HR closes the old entry, and indicates that the company moved to the new HR. Thus, we have to merge the two company entries, and the newer ID will be deleted. 
* We detect that the same company is registered with two HRs. Then probably we have two entries, and they will need to be merged.
* We found a company in the Bundesanzeiger, and couldn't find the company in the HR (e.g., because of inconsistent naming, say "A. Meier GmbH" in the Bundesanzeiger and "Anton Meier GmbH" in the HR. Later we figure out that they are the same company, and they will need to be merged.

## Appendix C: Company lifecycle and event types

Possible states in the lifecycle of a German company are:

State | German terminology | Explanation 
------|-------------|-------------
`Active` | Aktiv | normal state
`Liquidation` | "in Liquidation", "in Abwicklung" oder aufgelöst | Very limited operation in preparation of termination
`Terminated` | erloschen | Finally terminated

The liquidation state may be entered "regularly" by filing for liquidation, or otherwise, in the case of insolvency.
For legal details see https://de.wikipedia.org/wiki/Liquidation.

The following company event types are used in this API:

Event type | Explanation
-----------|-------------
`NewCompany` | Company was founded
`NameChange` | Company changed its name
`ManagementChange` | Change in the set of legal representatives (entry or leave)
`CapitalChange` | Nominal base capital of the company changed
`RegisterChange` | The Handelsregister id of the company changed 
`ContractChange` | The company contract ("Gesellschaftsvertrag") changed
`YearlyReport` | A new yearly report was filed (thus, a new set of financials will be available)
`AddressChange` | Adress changed
`LegalFormChange` | Legal form changed
`Insolvency` | A new insolvency process was opened
`InsolvencyChange` | A new publication concerning an ongoing insolvency process was filed
`MergerOrAcquisition` | The company was involved in a merger or acquisition 
`ControlChange` | The company's controlling company changed
`Liquidation` | The company entered the *Liquidation* state
`Continuation` |The company transitioned back to *Active* state 
`Termination` | The company was finally *Terminated* 
`Funding` | The company received a public funding
`Patent` | The company made a patent publication
`Trademark` | The company registered a trademark


## Appendix D: Publication sources

This appendix has been replaced by our online reference here:
https://www.northdata.com/_coverage

## Appendix E: Topic types

The following topic types are returned in "publication topics".

Group | Type | Note
--|--|--
`Change` | `Address` | Address
`Change` | `AnnouncementDemandedByLaw` | Announcement
`Change` | `BalanceDeadline` | Deadline for annual balance
`Change` | `Capital` | Capital
`Change` | `CollocationPlan` | Collocation plan and inventory
`Change` | `Continuation` | Continuation
`Change` | `Contract` | Articles of association
`Change` | `ConvertibleBonds` | Convertible bonds
`Change` | `Exit` | Termination
`Change` | `ExitAnnouncement` | Termination announcement
`Change` | `Headquarters` | Headquarters
`Change` | `IpDeletion` | Deletion
`Change` | `LegalForm` | Legal form
`Change` | `Liquidation` | Liquidation
`Change` | `ListOfBoardMembers` | List of board members
`Change` | `Minor` | 
`Change` | `Name` | Name
`Change` | `PersonalLaw` | Personal law
`Change` | `Proxy` | Proxy
`Change` | `Relation` | Relation
`Change` | `Restoration` | Restoration
`Change` | `Role` | Role
`Change` | `SegmentCode` | Segment classification
`Change` | `Statute` | Company statute
`Change` | `Subject` | Corporate Purpose
`Change` | `Subsidiary` | Subsidiary
`Entry` | `Entry` | Registration
`Insolvency proceedings` | `AbweisungMangelsMasse` | Refusal due to insufficient funds
`Insolvency proceedings` | `AnkuendigungRestschuldbefreiung` | Announcement of residual debt discharge
`Insolvency proceedings` | `AnmeldeFrist` | Entry Deadline
`Insolvency proceedings` | `Appeal` | Appeal
`Insolvency proceedings` | `AppealRejected` | Appeal rejected
`Insolvency proceedings` | `AuctionOfLand` | Auction of land
`Insolvency proceedings` | `Aufhebung` | Repeal
`Insolvency proceedings` | `CreditorsCommittee` | Creditors' committee
`Insolvency proceedings` | `Einstellung` | Suspension
`Insolvency proceedings` | `EinstellungMangelsMasse` | Refusal due to insufficient funds
`Insolvency proceedings` | `Eroeffnung` | Initiation
`Insolvency proceedings` | `InsolvenzSonstiges` | Misc.
`Insolvency proceedings` | `Insolvenzverwalter` | Insolvency administrator
`Insolvency proceedings` | `Masseunzulänglichkeit` | Insufficient assets
`Insolvency proceedings` | `Pruefungstermin` | Examination date
`Insolvency proceedings` | `ReceivershipTerminated` | Receivership terminated
`Insolvency proceedings` | `Restschuldbefreiung` | Residual debt discharge
`Insolvency proceedings` | `SchedulesOfClaims` | Schedules of claims
`Insolvency proceedings` | `Schlusstermin` | Closing date
`Insolvency proceedings` | `Sicherungsmassnahme` | Safeguards
`Insolvency proceedings` | `TerminationOrder` | Termination ordered
`Insolvency proceedings` | `TreuhaenderBestimmung` | Trustee
`Insolvency proceedings` | `Verguetung` | Compensation
`Insolvency proceedings` | `VerteilungsEntwurf` | Distribution proposal
`Insolvency proceedings` | `VerteilungsVerzeichnis` | Funds distribution
`Insolvency proceedings` | `VoluntaryArrangement` | Voluntary Arrangement
`Insolvency proceedings` | `VorlaufigeEroeffnung` | Provisional initiation
`Insolvency proceedings` | `VorlaufigerInsolvenzverwalter` | Preliminary insolvency administrator
`Insolvency proceedings` | `Zwangsverwaltung` | Receivership
`Intellectual Property` | `IpApplication` | Application
`Intellectual Property` | `IpOpposition` | Opposition
`Intellectual Property` | `IpRegistration` | Registration
`Intellectual Property` | `IpStatus` | Status
`Intellectual Property` | `IpSubject` | Subject
`Intellectual Property` | `IpType` | Type
`Public funding` | `FundingAmount` | Funding amount
`Public funding` | `FundingEnd` | End date
`Public funding` | `FundingProject` | Funded project
`Public funding` | `FundingStart` | Start date
`Yearly report` | `ActivityReport` | Activity report
`Yearly report` | `AnnualFinancialReport` | Annual report
`Yearly report` | `AuditorsReport` | Auditor's opinion
`Yearly report` | `Balance` | Balance sheet
`Yearly report` | `Earnings` | Earnings statement
`Yearly report` | `FinancialReportApproval` | Annual financial statement approval
`Yearly report` | `ProfitDistributionResolution` | Resolution on profit distribution or loss coverage

## Appendix F: Role types

Group | Type | Note
--|--|--
`Control` | `Beherrschung` | Control
`Control` | `Betriebspacht` | Company lease
`Control` | `Betriebsueberlassung` | Company lease
`Control` | `Branch` | Branch office
`Control` | `GeneralControl` | Control
`Control` | `Gewinngemeinschaft` | Profit pooling
`Control` | `Gewinnnabfuehrung` | Control
`Control` | `Hgb264` | Consolidation in parent company
`Control` | `Komplementaer` | General partner
`Control` | `Teilgewinnnabfuehrung` | Partial control
`Interest` | `DirectParent` | Direct parent
`Interest` | `IndirectParent` | Indirect parent
`Interest` | `Shareholder` | Shareholder
`Interest` | `VotingRights` | Voting rights
`Merger` | `Abspaltung` | Breakup
`Merger` | `AntitrustInvestigation` | AntitrustInvestigation
`Merger` | `Ausgliederung` | Spinoff
`Merger` | `Uebernahme` | Acquisition
`Merger` | `Verschmolzen` | Merger
`Legal representative` | `Abwickler` | Liquidator
`Legal representative` | `Actuary` | Actuary
`Legal representative` | `Auditor` | Auditor
`Legal representative` | `Aufsichtsrat` | Member of the Supervisory Board
`Legal representative` | `AuthorizedRepresentativeBody` | Authorized representative body
`Legal representative` | `AuthorizedSignatory` | Authorized signatory
`Legal representative` | `BranchOfficeManager` | Branch Office Manager
`Legal representative` | `ChairOfTheBoard` | Chair of the board
`Legal representative` | `DelegateOfTheBoard` | CEO
`Legal representative` | `DeputyDirector` | Deputy Director
`Legal representative` | `DesignatedMember` | Designated Member of the Board
`Legal representative` | `Director` | Director
`Legal representative` | `EmployeeRepresentative` | Employee representative
`Legal representative` | `Generaldirektor` | CEO
`Legal representative` | `GeschaeftsfuehrenderDirektor` | Managing Director
`Legal representative` | `Geschaeftsfuehrer` | Managing Director
`Legal representative` | `Handelnd` | Representative
`Legal representative` | `Inhaber` | Owner
`Legal representative` | `Liquidator` | Liquidator
`Legal representative` | `Manager` | Manager
`Legal representative` | `MemberOfTheBoard` | Member of the board
`Legal representative` | `NomineeDirector` | Nominee Director
`Legal representative` | `NomineeSecretary` | Nominee Secretary
`Legal representative` | `Notgeschaeftsfuehrer` | Temporary Managing Director
`Legal representative` | `Paechter` | Leaser
`Legal representative` | `Partner` | Partner
`Legal representative` | `PersoenlichHaftenderGesellschafter` | Fully liable partner
`Legal representative` | `President` | President
`Legal representative` | `Prokura` | Officer
`Legal representative` | `Secretary` | Secretary
`Legal representative` | `StaendigerVertreter` | Representative
`Legal representative` | `StellvertretenderGeneraldirektor` | Deputy CEO
`Legal representative` | `StellvertretenderGeschaeftsfuehrer` | Deputy Managing Director
`Legal representative` | `StellvertretenderVorstandsvorsitzender` | Deputy CEO
`Legal representative` | `Treasurer` | Treasurer
`Legal representative` | `ViceChairOfTheBoard` | Vice chair of the board
`Legal representative` | `ViceDirector` | Vice Director
`Legal representative` | `VicePresident` | Vice president
`Legal representative` | `Vorstand` | Member of the Executive Board
`Legal representative` | `Vorstandsvorsitzender` | CEO
`Succession` | `IdChange` | Register ID change
`Succession` | `Sitzverlegung` | Change of headquarters
`Succession` | `Umwandlung` | Change of legal form
`Other` | `CommonFiling` | 
`Other` | `FundingRecipient` | Funding recipient
`Other` | `InterestGroupMember` | Interest Group Member
`Other` | `IpOwner` | Owner
`Other` | `Publisher` | Publisher
`Other` | `SameAddress` | Address
`Other` | `SimilarName` | Similar name
`Other` | `Unknown` | unknown

## Appendix G: Event types

Group | Type | Note
--|--|--
`DEEP` | `Funding` | Public funding
`DEEP` | `IpDate` | IP date
`DEEP` | `Patent` | Patent
`DEEP` | `Trademark` | Trademark
`DEEP` | `YearlyReport` | Yearly report
`LEGAL` | `AddressChange` | Address change
`LEGAL` | `CapitalChange` | Capital change
`LEGAL` | `Continuation` | Continuation
`LEGAL` | `ContractChange` | Articles of association
`LEGAL` | `ControlChange` | Change of control
`LEGAL` | `Insolvency` | Insolvency filing
`LEGAL` | `InsolvencyChange` | Insolvency proceedings update
`LEGAL` | `LegalFormChange` | Legal form change
`LEGAL` | `Liquidation` | Liquidation
`LEGAL` | `MergerOrAcquisition` | Merger/Acquisition
`LEGAL` | `NameChange` | Name change
`LEGAL` | `NewCompany` | Registration
`LEGAL` | `OtherChange` | Misc. change
`LEGAL` | `RegisterChange` | Register change
`LEGAL` | `StatutoryChange` | Statutory change
`LEGAL` | `Termination` | Termination
`PEOPLE` | `ManagementChange` | Management change
`PEOPLE` | `SecondLevelPersonChange` | Management change
`PEOPLE` | `TopLevelPersonChange` | Management change