
# Data Services Guide to Snowflake Exports

North Data provides quarterly exports of its company data on Snowflake. Only our most popular country datasets are 
listed on the Snowflake marketplace and can be ordered directly (with free limited trials). 
Other country datasets are available on request. 

## Limited trials

The limited trials are restricted to a single city per country. 

## The Data Levels: M, L, XL

Exports are provided in three levels of different data "depth" with different pricing. The **M** level contains company data with the exception of related persons (e.g., legal representatives) and deep data (e.g. financial performance indicators). The **L** level adds related persons, and the **XL** levels adds the quantitative "deep" data. Our Snowflake datasets always provide the highest level 
available for each country.

Please note that actual data availability is different for every company. Availability depends on many different, mostly external factors. To better explore and understand the availability of our data, please use our trial data set and check our 
[country and sources coverage page](https://www.northdata.de/_coverage).

## Table data plus JSON

The data is provided in one large single table `COMPANY`. 
Its columns provide easy access to the most important data attributes and are listed below in (#columns).
In addition, the last column contains an extensive JSON representation with lots of additional information that is difficult to store in 
a table structure, including full history, related companies, and related person detail information. Documentation for the JSON
representation can be found in the [reference guide](https://northdata.github.io/doc/api/#Company).

## Table columns

Level | Column | Description
--|--|--
M | INTERNAL_ID | Internal ID
M | UNIQUE_KEY | Unique Key
M | REGISTER_ID | Register ID
M | EU_ID | EU ID
M | LEI | LEI
M | NAME | Name
M | LEGAL_FORM | Legal form
M | ELF_CODE | ELF code
M | STATUS | Status
M | COUNTRY | Country
M | POSTAL_CODE | Postal code
M | CITY | City
M | STREET | Street
M | ADDRESS_EXTRA | Address extra
M | LATITUDE | Latitude
M | LONGITUDE | Longitude
M | NORTH_DATA_URL | North Data URL
M | PROXY_POLICY | Proxy policy
M | SUBJECT | Subject
M | REGISTRATION | Registration
M | REGISTRATION_DATE | Registration date
M | TERMINATION | Termination
M | TERMINATION_DATE | Termination date
M | LIQUIDATION | Liquidation
M | LIQUIDATION_DATE | Liquidation date
M | INSOLVENCY_FILING | Insolvency filing
M | INSOLVENCY_FILING_DATE | Insolvency filing date
M | TICKER_SYMBOLS | Ticker Symbols
M | INDUSTRY_SEGMENT | Industry segment
M | SEGMENT_CODE | Segment code
M | PHONE | Phone
M | FAX | Fax
M | EMAIL | Email
M | WEBSITE | Website
M | VAT_ID | VAT Id
M | JSON | json
L | OFFICER_1 | Officer 1
L | ROLE_1 | Role 1
L | OFFICER_2 | Officer 2
L | ROLE_2 | Role 2
L | OFFICER_3 | Officer 3
L | ROLE_3 | Role 3
L | OFFICER_4 | Officer 4
L | ROLE_4 | Role 4
L | OFFICER_5 | Officer 5
L | ROLE_5 | Role 5
XL | FINANCIALS_DATE | Financials date
XL | BASE_SHARE_CAPITAL_EUR | Base/share capital EUR
XL | TOTAL_ASSETS_EUR | Total assets EUR
XL | EARNINGS_EUR | Earnings EUR
XL | EARNINGS_CAGR_PCT | Earnings CAGR %
XL | REVENUE_EUR | Revenue EUR
XL | REVENUE_CAGR_PCT | Revenue CAGR %
XL | RETURN_ON_SALES_PCT | Return on sales %
XL | EQUITY_EUR | Equity EUR
XL | EQUITY_RATIO_PCT | Equity ratio %
XL | RETURN_ON_EQUITY_PCT | Return on equity %
XL | EMPLOYEE_NUMBER | Employee number
XL | REVENUE_PER_EMPLOYEE_EUR | Revenue per employee EUR
XL | TAXES_EUR | Taxes EUR
XL | TAX_RATIO_PCT | Tax ratio %
XL | CASH_ON_HAND_EUR | Cash on hand EUR
XL | RECEIVABLES_EUR | Receivables EUR
XL | LIABILITIES_EUR | Liabilities EUR
XL | COST_OF_MATERIALS_EUR | Cost of materials EUR
XL | WAGES_AND_SALARIES_EUR | Wages and salaries EUR
XL | AVERAGE_SALARIES_PER_EMPLOYEE_EUR | Average salaries per employee EUR
XL | PENSION_PROVISIONS_EUR | Pension provisions EUR
XL | REAL_ESTATE_EUR | Real estate EUR
XL | AUDITOR | Auditor
XL | MKTG_TECH_REF_PERIOD | Mktg&Tech ref period
XL | NUMBER_OF_PUBLIC_FUNDINGS_PER_YEAR | Number of public fundings per year
XL | TOTAL_PUBLIC_FUNDING_PER_YEAR_EUR | Total public funding per year EUR
XL | PATENTS_PER_YEAR | Patents per year
XL | TRADEMARKS_PER_YEAR | Trademarks per year
All | JSON | Extended company data (History, related companies and person details)