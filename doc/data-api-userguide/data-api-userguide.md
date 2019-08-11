![image alt text](image_0.png)

# Data API User Guide

The data API allows you to directly access North Data's company database in JSON or XML format. 

Please see also:

- **Reference guide**: https://www.northdata.de/doc/api/index.html
- **Swagger / OpenAPI 2.0 definition file** https://github.com/northdata/api/blob/master/swagger.yaml
- **OpenAPI 3.0 definition file** https://github.com/northdata/api/blob/master/openapi.yaml

## Content

- [Quickstart](#quick-start)
  - [Example](#example)
  - [Response format](#response-format)
  - [Authentication](#authentication)
  - [Error handling](#error-handling)
  - [Language support](#language-support)
  - [Privacy protection](#privacy-protection)
- [Retrieving single companies](#retrieving-single-companies)
  - [Identifying a company by name and city](#identifying-a-company-by-name-and-city)
  - [Identifying a company by register ID and court city](#identifying-a-company-by-register-id-and-court-city)
  - [Identifying a company by internal company ID](#identifying-a-company-by-internal-company-id)
- [Accessing company detail information](#accessing-company-detail-information)
  - [Events](#events)
  - [Extras provided by third parties](#extras-provided-by-third-parties)
- [Retrieving persons](#retrieving-persons)
  - [Identifying a person by name, city and birth date](#identifying-a-person-by-name-city-and-birth-date)
  - [Identifying a person by internal person ID](#identifying-a-person-by-internal-person-id)
- [Selecting publications](#selecting-publications)
- [Searching](#searching)
  - [Retrieving large result lists](#retrieving-large-result-lists)
  - [Power Search](#power-search)
  - [Universal Search](#universal-search)
  - [Auto complete suggestions](#auto-complete-suggestions)
- [Appendix A: Database synchronization](#appendix-a-database-synchronization)
- [Appendix B: Company entry merger scenarios](#appendix-b-company-entry-merger-scenarios)
- [Appendix C: Company lifecycle and event types](#appendix-c-company-lifecycle-and-event-types)

## Quick start

An API key is required for using the Data API. To obtain an API key, please contact support@northdata.de.

### Example

The request:

https://www.northdata.de/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&api_key=XXXX_XXXX

will result in:

```json
{
  id: "57514825",
  name: {
    name: "1000mikes AG",
    legalForm: "AG"
  },
  address: {
    street: "Hansaplatz 4",
    postalCode: "20099",
    city: "Hamburg",
    country: "DE",
    lat: 53.5541139,
    lng: 10.0116615
  },
  register: {
    city: "Hamburg",
    id: "HRB 103038",
    uniqueKey: "12203550103038"
  },
  status: "liquidation"
}
```
If you use *get* requests, please ***make sure to properly encode parameters***. 

### Response format

The default response format is JSON. To enable XML responses add the parameter 
`output=xml`. The request:

https://www.northdata.de/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&api_key=XXXX_XXXX&output=xml

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

https://www.northdata.de/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&api_key=XXXX_XXXX

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
en | English (experimental)

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
retrieve company | https://www.northdata.de/_api/company/v1/company
retrieve publications of a company  | https://www.northdata.de/_api/company/v1/publications

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
retrieve company | https://www.northdata.de/_api/company/v1/company
power search | https://www.northdata.de/_api/search/v1/power
universal search | https://www.northdata.de/_api/search/v1/universal

will return only base data by default. 

If you want more detail, you may add one or more of the following parameters.

Parameter name | Type | Explanation
---------------|------|------------
`history` | boolean | true to include historical data
`financials` | boolean | true to include financial data 
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

https://www.northdata.de/_api/company/v1/company?address=Hamburg&name=1000MIKES%20AG&events=true&eventTypes=NameChange|AddressChange&maxEvents=3&api_key=XXXX_XXXX

### Extras provided by third parties

*Extras* are data items that cannot be determined from official company filings. We obtain them from third party data providers that evaluate sources such as company websites. Set the `extras` parameter to true in order to retrieve them,  

Extra Id | Explanation
---|---
phone | Company phone number 
fax | Fax number 
email | Email address
url | Website URL
vatId | VAT (value-added text) ID
wz | Industry code 

The industry code is the German [WZ 2008](https://de.wikipedia.org/wiki/Klassifikation_der_Wirtschaftszweige) code, which is is an extension of the EU standard [NACE rev. 2](https://en.wikipedia.org/wiki/Statistical_Classification_of_Economic_Activities_in_the_European_Community ) code.

## Retrieving persons

The following request is available to query a single person:

Request | URL
------- | ---------
retrieve person | https://www.northdata.de/_api/person/v1/person
retrieve publications associated with a person | https://www.northdata.de/_api/person/v1/publications

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

https://www.northdata.de/_api/person/v1/person?firstName=Alfons&lastName=Schuhbeck&address=M%C3%BCnchen&birthDate=1949-05-02

### Identifying a person by internal person ID

Persons might also be identified using a `personId`. This is strongly discouraged, because there are various scenarios where this ID may change overtime. Please contact us for discussion if you need more detail on this.

## Selecting publications

The followinging request are available to retrieve publications:

Request | URL
------- | ---------
query publications | https://www.northdata.de/_api/pub/v1/publications
retrieve publications associated with a company | https://www.northdata.de/_api/company/v1/publications
retrieve publications associated with a person | https://www.northdata.de/_api/person/v1/publications

These requests share several parameters:

Parameter name | Type | Explanation
---------------|------|------------
`sources` | string array | restrict which sources are allowed for the returned publications 
`content` | boolean | whether to include publication text / html 

The default for the `sources` parameter is `sources=Hrb|Eb|Ins` (possible values and their definitions can be found in the reference guide).

## Searching 

The following requests are available for searching:

Request | URL
------- | ---------
power search | https://www.northdata.de/_api/search/v1/power
universal search | https://www.northdata.de/_api/search/v1/universal
suggest (auto complete) | https://www.northdata.de/_api/search/v1/suggest

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

Parameter name | Type | Explanation
---------------|------|------------
`name` | string | company name or empty
`description` | string | wz code or keywords to match in the company subject
`address` | string | address (any level of precision, from house to country)
`maxDistanceKm` | number | maximum distance from given address 
`status` | string array | list of valid statuses (active, terminated, liquidation)
`financialId`  | string array | list of financial ids for financial filtering
`lowerBound` | number array | list of lower bounds for financial filterings
`upperBound` | number array | list of upper bounds for financial filterings
`eventType`  | string array | list of event types for event filtering (see [Appendix C](#appendix-c-company-lifecycle-and-event-types))
`minDate`  | date array | list of minimum dates for event filtering
`maxDate`  | date array | list of maximum dates for event filtering

Please note that all the [parameters for accessing company detail information](#accessing-company-detail-information) may be used. 

#### The `description` parameter 

The description parameter may be a partial or full industry code (see above) or one or more keywords to match in the company subject.

```
description=62.03.0
description=62.03
description=Immobilien
```

#### Filtering by financials

Multiple ranges may be specified. For example, to filter for companies that had earnings in the range of €1 - €2M and 50 - 100 employees, use the following parameters:

```
financialId=Earnings|Employees
lowerBound=1000000|50
upperBound=2000000|100
```
A list of financials is provided [here](https://www.northdata.de/_financials).
For open (unlimited) ranges just omit the number. 
The last known financials are used for filtering.

The search results will match all filters (although some programming languages use the '|' character for *"or"* operations, here it is a simple separator and the the filters have *"and"* semantics.)

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

### Universal Search

The universal search tries to provide a best interpretation for the given search string. 

Parameter name | Type | Explanation
---------------|------|------------
`query` | string | the search string
`domain` | string | "company", "person" or empty to match any
`status` | string array | list of valid statuses (active, terminated, liquidation)

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

Parameter name | Type | Explanation
---------------|------|------------
`query` | string | the search string
`limit` | number | maximum number of results to be return
`domain` | string | "company", "person" or empty to match both

Please note that the parameters for accessing company detail information may **not** be used. 

## Appendix A: Database synchronization

This appendix describes how external databases may be kept in sync using regular updates after an export. 

Please use the publications method of the Data API (link to the reference guide)
https://www.northdata.de/doc/api/index.html#pubV1PublicationsGet

Example (please use your API key and adjust the dates):

https://www.northdata.de/_api/pub/v1/publications?minTimestamp=2017-03-14&maxTimestamp=2017-03-15&sources=Hrb|Eb|Ins|Nd&apiKey=XXXX-XXXX

This method provides all publications of a particular day. We recommend to run a job every day in the early morning to fetch the publications of the previous day. Each publication represents one or multiple events that may result in changes of company data. The response of the API request provides an array of publications. For each publication there is a field publisher, which contains the updated company data. 

In addition, you may want to set the parameters `publisherFinancials`, `publisherSheets`, `publisherRelations`, `publisherHistory`, and/or `publisherEvents` to true in order to retrieve company detail information. 

Please note that the daily number of publications is too high to fit in a single HTTPS call. (Expect up to 2.000 HR publications, 2.000 Bundesanzeiger publications, 5.000 insolvency publications a day.) Therefore, you should use a loop to fetch all the publications:

1. Set the parameters `minTimestamp` and `maxTimestamp` for the requested time period (recommendation: `minTimestamp` to the previous day and `maxTimestamp` current day. If you omit the time, 0:00 is assumed)
1. Invoke HTTPS API method
1. In case of an HTTP 503 status (service unavailable) retry the call up to two or three times
For all publications the JSON response has a field publisher with the updated company data  -> write this back, but see also below
1. Read the `newPos` field:
   *  not empty -> append as parameter `pos` to the request, continue with step 2
   *  empty -> complete!
1. Looking up company data in your database

It is important **not** to rely on the internal IDs that we provide via the API and the export. The IDs are only valid temporarily and may change over time. There are various real world cases that require us to merge or split company records resulting in changes of the ID. 

Instead, we provide a field `uniqueKey` in `company.registe`r which may serve to safely identify a company. Every company has a current register and a list of historical registers. The best method to retrieve companies is to keep a table that maps registers to companies.

HR_UNIQUE_KEY | YOUR_INTERNAL_COMPANY_ID
--------------|----------------------
21478278742 | company1
89849894894 | company2
38738738733 | company2
 
Please note that you will infrequently encounter company data that has no register or unique key at all. This happens, and unfortunately there is no way to uniquely identify these companies. For example, there may be “Restaurant Shanghai, Berlin” filing insolvency. There is no way to safely match it. it In this case, you may either (1) drop the update, (2) just append it to your database, (3) solve the case manually.  

## Appendix B: Company entry merger scenarios

Sometimes, North Data needs to merge company entries. The consequence is that internal company IDs may change over time. ***You do not need to know or worry about why this is the case.*** But, we are frequently are asked for the reasons, so here they are.

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










