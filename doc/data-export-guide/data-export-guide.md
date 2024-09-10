
# Data Services Guide to Quarterly Exports

North Data provides quarterly exports of its complete collection of company data in many different variants. This guide explains how to select and download the right package for you.

## Table of contents
- [Data Services Guide to Quarterly Exports](#data-services-guide-to-quarterly-exports)
  - [Table of contents](#table-of-contents)
  - [Test Data](#test-data)
  - [Choosing a Format: CSV versus JSONL](#choosing-a-format-csv-versus-jsonl)
  - [The Data Levels: M, L, XL](#the-data-levels-m-l-xl)
  - [Navigating the download folders](#navigating-the-download-folders)
  - [Split downloads](#split-downloads)
  - [Alternative modes of access](#alternative-modes-of-access)
  - [JSON data contents](#json-data-contents)
  - [CSV data contents](#csv-data-contents)

## Test Data

We provide test data that looks exactly like the actual quarterly export, though restricted to a single city per country. To access the test data, please contact our team and they will provide you with a link or access credentials for our files server at https://files.northdata.com.

It is highly recommended to familiarize yourself with the test data to understand its extent, and which package suits your needs.

## Choosing a Format: CSV versus JSONL

Exports are provided in two different formats.

 - **JSONL** (JSON separated with new lines). This is the right choice if you intend to process the data with a programming language. Also, in case you already work with our API, this would be the most fitting option, as it has the same format.

 - **CSV** (Comma-separated value) with commas as separator and UTF-8 as encoding. This would be the more suitable format in case you would like to process the data with a data analytics tool.
    It’s important to mention that, while in theory it is possible to open the CSV files in Excel (and works well for the test data), the big data sets might surpass Excel’s data processing capabilities.
   
Note that the JSONL editions contain more detail than CSV editions. This is due to restrictions of the CSV format, which does not support nesting. See below in [CSV data contents](#csv-data-contents).

## The Data Levels: M, L, XL

Exports are provided in three levels of different data "depth" with different pricing. The **M** level contains all company data with the exception of related persons (e.g., legal representatives) and deep data (e.g. financial performance indicators). The **L** level adds related persons, and the **XL** levels adds the quantitative "deep" data.

## Navigating the download folders

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

The zip file name tells you the date of generation, the city name (for test data only), the country, the level (M, L, XL) and the format (CSV or JSONL) and the language (en for English and de for German). The language choice only makes a difference for descriptive fields, for example you get translations (managing director - Geschäftsführer).

## Split downloads

Sometimes it is challenging to deal with the large size of the individual data files. For this reason, we provide a so-called “split variant” for the full quarterly exports. For instance, if you open the `at-m` full data folder (which will be available to you after purchasing a subscription), you will find 8 variants to choose from:

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

## Alternative modes of access

Although the recommended way to access the export files is to download them via HTTPS (e.g., with a web browser or curl), there are many other alternatives that you might prefer:

 * [FTP](https://www.files.com/integrations/ftp-any-provider)
 * [SFTP](https://www.files.com/integrations/sftp-any-provider)
 * [WevDAV](https://www.files.com/integrations/webdav)
 * [API](https://developers.files.com), including SDKs for Javascript, Ruby, PHP, .NET, Python, Java and Go.
 * [Desktop App](https://www.files.com/docs/features/desktop-app) for Mac and Windows 
 
## JSON data contents

The JSON data provides all data fields (including history) in the same format as our API. See the API documentation here:
https://northdata.github.io/doc/api/#Company

Please note that actual data availability is different for every company. Availability depends on many different, mostly external factors. To better explore and understand the availability of our data, please use our test data.

## CSV data contents

The CSV data is provided as a single large table. Values are separated by commas, and the encoding is UTF-8. (In many data analytics tools, you will need to configure these as import settings.)

Due to this structure, it comes with various restrictions as opposed to the JSON data: the number of legal representatives is limited to 5, there is no history, and relations to other companies are not contained. Below, the table headers for the CSV data are shown.

As previously mentioned, please note that actual data availability is different for every company.

Level | English | German | French
--|--|--|--
M | Internal ID | Interne ID | ID interne
M | Unique Key | Unique Key | Unique Key
M | Register ID | Register ID | ID du Régistre
M | LEI | LEI | LEI
M | Name | Name | Dénomination
M | Legal form | Rechtsform | Forme juridique
M | ELF code | ELF-Code | Code ELF
M | Status | Status | État
M | Country | Land | Pay
M | Postal code | PLZ | Code postal
M | City | Ort | Lieu
M | Street | Straße | Rue
M | Address extra | Adress-Extra | Adresse Extra
M | Latitude | Latitude | Latitude
M | Longitude | Longitude | Longitude
M | North Data URL | North Data URL | URL North Data
M | Proxy policy | Vertretungsregelung | Règle de représentation
M | Subject | Gegenstand | Objet social
M | Industry segment | Branche | Secteur d’activité
M | Segment code | Branchencode | Code de secteur
M | Phone | Tel. | Tél.
M | Fax | Fax | Fax
M | Email | E-Mail | Email
M | Website | Website | Site web
M | VAT Id | USt.-Id. | N° de TVA
L | Officer 1 | GF / Vst. 1 | Repr. légal 1
L | Role 1 | Rolle 1 | Rôle 1
L | Officer 2 | GF / Vst. 2 | Repr. légal 2
L | Role 2 | Rolle 2 | Rôle 2
L | Officer 3 | GF / Vst. 3 | Repr. légal 3
L | Role 3 | Rolle 3 | Rôle 3
L | Officer 4 | GF / Vst. 4 | Repr. légal 4
L | Role 4 | Rolle 4 | Rôle 4
L | Officer 5 | GF / Vst. 5 | Repr. légal 5
L | Role 5 | Rolle 5 | Rôle 5
XL | Financials date | Finanzkennzahlen Datum | Date des indicateurs
XL | Base/share capital EUR | Stamm-/Grundkapital EUR | Capital social / capital de base EUR
XL | Total assets EUR | Bilanzsumme EUR | Bilan total EUR
XL | Earnings EUR | Gewinn EUR | Profit EUR
XL | Earnings CAGR % | Gewinn CAGR % | Profit CAGR %
XL | Revenue EUR | Umsatz EUR | Chiffre d’affaires EUR
XL | Revenue CAGR % | Umsatz CAGR % | Chiffre d’affaires CAGR %
XL | Return on sales % | Umsatzrendite % | Rendement sur chiffre d’affaires %
XL | Equity EUR | Eigenkapital EUR | Capital propre EUR
XL | Equity ratio % | EK-Quote % | Ratio d’équité %
XL | Return on equity % | EK-Rendite % | Rendement des capitaux propres %
XL | Employee number | Mitarbeiterzahl | Nombre d’employés
XL | Revenue per employee EUR | Umsatz pro Mitarbeiter EUR | Chiffre d’affaires par employé EUR
XL | Taxes EUR | Steuern EUR | Impôts EUR
XL | Tax ratio % | Steuer-Quote % | Taux d'imposition %
XL | Cash on hand EUR | Kassenbestand EUR | Solde trésorerie EUR
XL | Receivables EUR | Forderungen EUR | Créances EUR
XL | Liabilities EUR | Verbindlichkeiten EUR | Dettes EUR
XL | Cost of materials EUR | Materialaufwand EUR | Dépenses matérielles EUR
XL | Wages and salaries EUR | Personalaufwand EUR | Dépenses personnelles EUR
XL | Average salaries per employee EUR | Personalaufwand pro Mitarbeiter EUR | Charges de personnel par employé EUR
XL | Pension provisions EUR | Pensionsrückstellungen EUR | Provisions de pensions EUR
XL | Real estate EUR | Immobilien und Grundstücke EUR | L’immobilier EUR
XL | Auditor | Wirtschaftsprüfer | Auditeur
XL | Mktg&Tech ref period | Mktg&Tech Bezugszeitraum | Période de référence Mktg & Tech
XL | Number of public fundings per year | Anzahl Förderungen pro Jahr | Nombre de financements publics par an
XL | Total public funding per year EUR | Gesamtfördersumme pro Jahr EUR | Financement public par an EUR
XL | Patents per year | Patente pro Jahr | Brevets par an
XL | Trademarks per year | Marken pro Jahr | Marques par an
