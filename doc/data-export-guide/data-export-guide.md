
# Data Services Guide to Quarterly Exports

North Data provides full quarterly exports of its company data in many different variants. This guide explains how to select and download the right package for you.

- [Data Services Guide to Quarterly Exports](#data-services-guide-to-quarterly-exports)
  - [Test Data](#test-data)
  - [Choosing a Format: CSV versus JSONL](#choosing-a-format-csv-versus-jsonl)
  - [The Level: M versus L versus XL](#the-level-m-versus-l-versus-xl)
  - [Navigating the Download Folders](#navigating-the-download-folders)
  - [Split downloads](#split-downloads)
  - [Alternative modes of accessing the export files](#alternative-modes-of-accessing-the-export-files)
  - [JSON data contents](#json-data-contents)
  - [CSV data contents](#csv-data-contents)

## Test Data

We provide test data that looks exactly like the actual quarterly export, but is restricted to a single city per country. To access the test data, please contact our team and they will provide you with a link or access credentials for our files server at https://files.northdata.com.

It is highly recommended to familiarize yourself with the test data to understand the extent of data, and which package suits your needs.

## Choosing a Format: CSV versus JSONL

Exports are provided in two different formats.

 - **JSONL** (JSON separated with new lines). This is the right choice if you intend to process the data with a programming language. It is the same format as our API, so if you also work with the API, you should choose this. 

 - **CSV** (Comma-separated value). This is the right choice if you want to open and process the data with a tool.
   Please note that while in theory it is possible to open the CSV files in Excel (and works well for the test data), the big data sets might be too large to work with Excel.
   
Note that the JSONL editions contain more detail than CSV editions. This is due to restrictions of the CSV format, which does not support nesting. See below in [CSV data contents](#csv-data-contents).

## The Level: M versus L versus XL

Exports are provided in three levels of different data "depth" with different pricing. The **M** level contains all company data with the exception of related persons (e.g., legal representatives) and deep data (e.g. financial performance indicators). The **L** level adds related persons, and the **XL** levels adds the quantitave "deep" data.

## Navigating the Download Folders

After opening the link provided by our team or logging into our files server, you'll find a directory structure looking like this:

```
- at-m
- at-l
- at-xl
- be-m
- be-l
etc.
```

The first two letters signify the country (AT for Austria, BE for Belgium, and so on), and the other letters the level (M, L, or XL). 

For example, if you open the `at-m` test data folder you'll find the following zip files:

```
testdata20230630-innsbruck-AT-M-de.csv.zip
testdata20230630-innsbruck-AT-M-de.jsonl.zip
testdata20230630-innsbruck-AT-M-en.csv.zip
testdata20230630-innsbruck-AT-M-en.jsonl.zip
```

The zip file name tells you the date of generation, the city name (for test data only), the country, the level (M, L, XL) and the format (CSV or JSONL) and the language (`en` for English and `de` for German). The language choice only makes a difference for descriptive fields, for example you get translations (managing director - Geschäftsführer). 

## Split downloads

Sometimes it is cumbersome to deal with the large size of the individual data files. For this reason, we provide a so-called split variant for the full quarterly exports. For example, if you open the `at-m` full data folder (which will be available to you after purchasing a subscription), you'll find 8 variants to choose from:

```
export2023Q3-AT-M-de.csv.zip
export2023Q3-AT-M-de.jsonl.zip
export2023Q3-AT-M-en.csv.zip
export2023Q3-AT-M-en.jsonl.zip
export2023Q3-AT-M-de.csv.split.zip
export2023Q3-AT-M-de.jsonl.split.zip
export2023Q3-AT-M-en.csv.split.zip
export2023Q3-AT-M-en.jsonl.split.zip
```

This allows you to choose a format, a language, and optionally a split variant. In the split variant, Instead of one big data file, the zip file contains up to 25 smaller files that you may process separately. 

## Alternative modes of accessing the export files

The recommended way to access the export files is to download them with a browser (i.e, via HTTPS). But there are many more options:
 * [FTP](https://www.files.com/integrations/ftp-any-provider)
 * [SFTP](https://www.files.com/integrations/sftp-any-provider)
 * [WevDAV](https://www.files.com/integrations/webdav)
 * [API](https://developers.files.com), including SDKs for Javascript, Ruby, PHP, .NET, Python, Java and Go.
 * [Desktop App](https://www.files.com/docs/features/desktop-app) for Mac and Windows 
 
## JSON data contents

The JSON data provides all data fields (including history) in the same format as our API, see the API documentation here:
https://northdata.github.io/doc/api/#Company

Please note that actual data availability is different for every company. Availability depends on many different, mostly external factors. To explore availability, please use our test data. 

## CSV data contents

The CSV data is provided as a single large table. Due to this structure, it has various restrictions compared to the JSON data: the number of legal representatives is limited to 5, there is no history, and relations to other companies are not contained.
Below, the table headers for the CSV data are shown.

Again, please note that actual data availability is different for every company. Availability depends on many different, mostly external factors. To explore availability, please use our test data. 

Level | English | German
--|--|--
M | Internal ID | Interne ID
M | Unique Key | Unique Key
M | Register ID | Register ID
M | LEI | LEI
M | Name | Name
M | Legal form | Rechtsform
M | ELF code | ELF-Code
M | Status | Status
M | Country | Land
M | Postal code | PLZ
M | City | Ort
M | Street | Straße
M | Address extra | Adress-Extra
M | Latitude | Latitude
M | Longitude | Longitude
M | North Data URL | North Data URL
M | Proxy Policy | Vertretungsregelung
M | Subject | Gegenstand
M | Industry segment | Branche
M | Segment code | Branchencode
M | Phone | Tel.
M | Fax | Fax
M | Email | E-Mail
M | Website | Website
M | VAT Id | USt.-Id.
L | Officer 1 | GF / Vst. 1
L | Role 1 | Rolle 1
L | Officer 2 | GF / Vst. 2
L | Role 2 | Rolle 2
L | Officer 3 | GF / Vst. 3
L | Role 3 | Rolle 3
L | Officer 4 | GF / Vst. 4
L | Role 4 | Rolle 4
L | Officer 5 | GF / Vst. 5
L | Role 5 | Rolle 5
XL | Financials date | Finanzkennzahlen Datum
XL | Base/share capital EUR | Stamm-/Grundkapital EUR
XL | Total assets EUR | Bilanzsumme EUR
XL | Earnings EUR | Gewinn EUR
XL | Earnings CAGR % | Gewinn CAGR %
XL | Revenue EUR | Umsatz EUR
XL | Revenue CAGR % | Umsatz CAGR %
XL | Return on sales % | Umsatzrendite %
XL | Equity EUR | Eigenkapital EUR
XL | Equity ratio % | EK-Quote %
XL | Return on equity % | EK-Rendite %
XL | Employee number | Mitarbeiterzahl
XL | Revenue per employee EUR | Umsatz pro Mitarbeiter EUR
XL | Taxes EUR | Steuern EUR
XL | Tax ratio % | Steuer-Quote %
XL | Cash on hand EUR | Kassenbestand EUR
XL | Receivables EUR | Forderungen EUR
XL | Liabilities EUR | Verbindlichkeiten EUR
XL | Cost of materials EUR | Materialaufwand EUR
XL | Wages and salaries EUR | Personalaufwand EUR
XL | Average salaries per employee EUR | Personalaufwand pro Mitarbeiter EUR
XL | Pension provisions EUR | Pensionsrückstellungen EUR
XL | Real estate EUR | Immobilien und Grundstücke EUR
XL | Auditor | Wirtschaftsprüfer
XL | Mktg&Tech ref period | Mktg&Tech Bezugszeitraum
XL | Number of public fundings per year | Anzahl Förderungen pro Jahr
XL | Total public funding per year EUR | Gesamtfördersumme pro Jahr EUR
XL | Patents per year | Patente pro Jahr
XL | Trademarks per year | Marken pro Jahr
