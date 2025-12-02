# PxWebApi 2 user guide

This guide describes how the PxWebApi works in general. Note that each
organization that installs PxWebApi to make their database accessible can
configure the API to suit their own needs by setting different limits, licenses,
languages, or rate controls according to their own policies.

!!! note "Examples"
    Examples in this guide will be done against Statistic Norways API for thiere
    Statbank.

See also the [OpenAPI specifications for the PxWebApi 2](https://github.com/PxTools/PxApiSpecs)
at GitHub.

## Basic concepts

The PxWebApi uses some abstractions to represent the different parts of the
statistical database. This section aims to give the reader a better
understanding of those abstractions laying a foundation for understanding the
structure of the API and the data that it provides.

### Table

A table is the representation of a statistical table. Others may sometimes refer
to the same concept a dataset, cube, or multi-dimensional cube. But we have
chose to call it a table due to historical reasons. A table consists of two
parts the data that contains the numbers and the metadata that gives the data a
context/meaning. E.g. data 10 452 326 and the metadata the population of Sweden
December 31 2021.

### Database

A database is a collection of tables that are organized and structured together
according to some schema.

A common mistake is to mix the database concept with a relational database
management system like Oracle Database or Microsoft SQL Server.

Also sometimes when referring to the database one might refer to the instance of
the database.

### Folder

Folders are used to organize tables in a database. A folder can contain other
folders or tables. Usually, the first level of folders in a database are referred
to the subject areas of the database. Others might also call them the themes of
the database.

### Variable

Tables are multidimensional and variables are the concept used to describe the
data (others may refer to variables as dimensions).  Take the example data
10 452 326 and the metadata the population of Sweden December 31 2021. 10 452 326
is described by tree variables

1. What we are measuring, in our case this is the population. This variable is
   special and is referred to as the content variable. There and only be one
   content variable. Sometimes when there is just one content the content
   variable may be omitted instead the context is given by other metadata for
   the table.
2. The point in time that the number is associated to in our case 31 December
   2021. This variable type is also special and is referred to as the time
   variable. There should always be one and only one time variable.
3. The region. In our case it is Sweden. This is also a special kind of variable
   called a geographical variable. A table might have zero or many geographical
   variables but usually only have one if they have any.

There is a fourth kind of variable that is just called a variable that is used
to describe the data. Imagen, that we had divided 10 452 326 by gender so we
have two data cells instead 5 260 707 and 5 191 619 one for male and one for
female. Then that fourth variable would be gender.

### Value

Variables have distinct values that make up the space for it. E.g. our gender
variable above have two values one for male and one for female.

### Codelist

A variable might have code list associated to that defined a new space for the
variable by providing different sets of values. E.g. imagine, you have a regional
variable with values for each municipality in Sweden. Then you might have a
codelist that transforms the municipalities into counties.

### Data cell

A data cell is the individual measure in a table.
The total number of cells for a table is given by the product of the number of
values for each variable.

## Finding tables

You can find tables via the API by hiting the `tables` endpoint. E.g. you can
get the tables in english for Statistics Norways databas with the following URL

> [https://data.ssb.no/api/pxwebapi/v2/tables?lang=en](https://data.ssb.no/api/pxwebapi/v2/tables?lang=en)

This will return the following response

```json
{
    "language": "en",
    "tables": [
        {
            "id": "13760",
            "label": "13760: Labour force, employment, unemployment and man-weeks worked, by sex and age. Break and seasonally adjusted figures 2006M01-2025M09",
            "description": "",
            "updated": "2025-10-23T06:00:00Z",
            "firstPeriod": "2006M01",
            "lastPeriod": "2025M09",
            "category": "public",
            "variableNames": [
                "gender",
                "age",
                "type of adjustment",
                "contents",
                "month"
            ],
            "source": "Statistics Norway",
            "subjectCode": "al",
            "timeUnit": "Monthly",
            "paths": [
                [
                    {
                        "id": "al",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "al03",
                        "label": "Unemployment"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ],
                [
                    {
                        "id": "al",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "al06",
                        "label": "Employment"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ],
                [
                    {
                        "id": "in",
                        "label": "Immigration and immigrants"
                    },
                    {
                        "id": "in01",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ]
            ],
            "links": [
                {
                    "rel": "self",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13760?lang=en"
                },
                {
                    "rel": "alternate",
                    "hreflang": "no",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13760?lang=no"
                },
                {
                    "rel": "metadata",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13760/metadata?lang=en"
                },
                {
                    "rel": "data",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13760/data?lang=en&outputFormat=json-stat2"
                }
            ]
        },
        {
            "id": "14483",
            "label": "14483: Population, by labour force status, age and sex. Break and seasonally adjusted figures 2009K1-2025K3",
            "description": "",
            "updated": "2025-11-06T07:00:00Z",
            "firstPeriod": "2009K1",
            "lastPeriod": "2025K3",
            "category": "public",
            "variableNames": [
                "gender",
                "age",
                "type of adjustment",
                "contents",
                "quarter"
            ],
            "source": "Statistics Norway",
            "subjectCode": "al",
            "timeUnit": "Quarterly",
            "paths": [
                [
                    {
                        "id": "al",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "al03",
                        "label": "Unemployment"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ],
                [
                    {
                        "id": "al",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "al06",
                        "label": "Employment"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ],
                [
                    {
                        "id": "in",
                        "label": "Immigration and immigrants"
                    },
                    {
                        "id": "in01",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ]
            ],
            "links": [
                {
                    "rel": "self",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/14483?lang=en"
                },
                {
                    "rel": "alternate",
                    "hreflang": "no",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/14483?lang=no"
                },
                {
                    "rel": "metadata",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/14483/metadata?lang=en"
                },
                {
                    "rel": "data",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/14483/data?lang=en&outputFormat=json-stat2"
                }
            ]
        },
        {
            "id": "13618",
            "label": "13618: Population, by labour force status, age and sex. Break adjusted figures 2009-2024",
            "description": "",
            "updated": "2025-02-10T07:00:00Z",
            "firstPeriod": "2009",
            "lastPeriod": "2024",
            "category": "public",
            "variableNames": [
                "labour force status",
                "gender",
                "age",
                "contents",
                "year"
            ],
            "source": "Statistics Norway",
            "subjectCode": "al",
            "timeUnit": "Annual",
            "paths": [
                [
                    {
                        "id": "al",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "al03",
                        "label": "Unemployment"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ],
                [
                    {
                        "id": "al",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "al06",
                        "label": "Employment"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ],
                [
                    {
                        "id": "in",
                        "label": "Immigration and immigrants"
                    },
                    {
                        "id": "in01",
                        "label": "Labour market and earnings"
                    },
                    {
                        "id": "aku",
                        "label": "Labour force survey"
                    }
                ]
            ],
            "links": [
                {
                    "rel": "self",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13618?lang=en"
                },
                {
                    "rel": "alternate",
                    "hreflang": "no",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13618?lang=no"
                },
                {
                    "rel": "metadata",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13618/metadata?lang=en"
                },
                {
                    "rel": "data",
                    "hreflang": "en",
                    "href": "https://data.ssb.no/api/pxwebapi/v2/tables/13618/data?lang=en&outputFormat=json-stat2"
                }
            ]
        }
    ],
    "page": {
        "pageNumber": 1,
        "pageSize": 3,
        "totalElements": 3877,
        "totalPages": 1293,
        "links": [
            {
                "rel": "next",
                "hreflang": "en",
                "href": "https://data.ssb.no/api/pxwebapi/v2/tables/?lang=en&pagesize=3&pageNumber=2"
            },
            {
                "rel": "last",
                "hreflang": "en",
                "href": "https://data.ssb.no/api/pxwebapi/v2/tables/?lang=en&pagesize=3&pageNumber=1293"
            }
        ]
    },
    "links": [
        {
            "rel": "self",
            "hreflang": "en",
            "href": "https://data.ssb.no/api/pxwebapi/v2/tables/?lang=en&pagesize=3&pageNumber=1"
        }
    ]
}
```

The response will contain a list of tables that are in the database in that
specific language. It also conatins information about result pages. The hole list
is not returned in one response instead the list is split up in pages containing
subparts of the complete list of tables.

The `tables` endpoint can take the following optional arguments to filter and
select a specific slice of the resulting list of tables:

- `lang` an laguage parameter specifing which language the result should be in.
- `query` an query string that can filter out tables.
- `pastDays` filters out tables in the result so only tables updated since the
   number of `pastDays`.
- `includeDiscontined` if discontined tables should be included or not in the result.
- `pageSize` All tables might not be included in the response depending on how
   many tables there is in the result and the `pageSize` sets how many tables
   are returned in the response. The default value might be changed by the organization.
- `pageNumber`  The `pageNumber` specifies which page sould be in the response. The
  default is the first page.

### Examples

#### Search for "pizza" in all tables

Add the `query` parameter to the url like

> <https://data.ssb.no/api/pxwebapi/v2/tables?lang=en&query=pizza>

#### Search for not discontinued tables where also containing the term "children"

Add the `includeDiscontinued` query parameter like

> <https://data.ssb.no/api/pxwebapi/v2/tables?lang=en&query=children&includeDiscontinued=true>

#### Search for tables that have been recently updated

Use the parameter `pastDays` if you want to find tables that have been updated like

> <https://data.ssb.no/api/pxwebapi/v2/tables?pastdays=3&lang=en>

## Basic information about the table

Basic information about the table can be retrieved with a URL like this `tables/{TABLE-ID}`

Example from Statistics Norway for table with `id` *05810* would be

> [https://data.ssb.no/api/pxwebapi/v2/tables/05810?lang=en](https://data.ssb.no/api/pxwebapi/v2/tables/05810?lang=en)

This would give us the following result

```json
{
    "language": "en",
    "id": "05810",
    "label": "05810: Population, by sex and age 1845-2025",
    "description": "",
    "sortCode": "000440",
    "updated": "2025-02-25T07:00:00Z",
    "firstPeriod": "1845",
    "lastPeriod": "2025",
    "category": "public",
    "variableNames": [
        "gender",
        "age",
        "contents",
        "year"
    ],
    "source": "Statistics Norway",
    "subjectCode": "be",
    "timeUnit": "Annual",
    "paths": [
        [
            {
                "id": "be",
                "label": "Population"
            },
            {
                "id": "be01",
                "label": "Children, families and households"
            },
            {
                "id": "folkemengde",
                "label": "Population"
            }
        ],
        [
            {
                "id": "be",
                "label": "Population"
            },
            {
                "id": "be05",
                "label": "Population count"
            },
            {
                "id": "folkemengde",
                "label": "Population"
            }
        ],
        [
            {
                "id": "in",
                "label": "Immigration and immigrants"
            },
            {
                "id": "in03",
                "label": "Population"
            },
            {
                "id": "folkemengde",
                "label": "Population"
            }
        ]
    ],
    "links": [
        {
            "rel": "self",
            "hreflang": "en",
            "href": "https://data.ssb.no/api/pxwebapi/v2/tables/05810?lang=en"
        },
        {
            "rel": "alternate",
            "hreflang": "no",
            "href": "https://data.ssb.no/api/pxwebapi/v2/tables/05810?lang=no"
        },
        {
            "rel": "metadata",
            "hreflang": "en",
            "href": "https://data.ssb.no/api/pxwebapi/v2/tables/05810/metadata?lang=en"
        },
        {
            "rel": "data",
            "hreflang": "en",
            "href": "https://data.ssb.no/api/pxwebapi/v2/tables/05810/data?lang=en&outputFormat=json-stat2"
        }
    ]
}
```

Here you can see among other things the table title (`label`), the first and
latest period in the time series (`firstPeriod` and `lastPeriod`), and which
variables are included (`variableNames`). `Paths` shows the table's location(s) in
the subject structure.

## Metadata for the table

Detailed metadata for a given table can be retrieved using he endpoint `tables/{TABLE-ID}/metadata`.
The metadata is displayed in the JSON-stat v2 format.

!!! note
    You will need to read the metadata to be able to construct queries for
    extracting data.

Example from Statistics Norway table with *id* 05810

> [https://data.ssb.no/api/pxwebapi/v2/tables/05810/metadata?lang=en](https://data.ssb.no/api/pxwebapi/v2/tables/05810/metadata?lang=en)

This would result in the following response.

```json
{
    "version": "2.0",
    "class": "dataset",
    "label": "05810: Population, by sex and age 1845-2025",
    "source": "Statistics Norway",
    "updated": "2025-02-25T07:00:00Z",
    "note": [
        "Until 1990 the figures correspond per 31 Desember. As from 1995 the figures correspond per 1 January."
    ],
    "role": {
        "time": [
            "Tid"
        ],
        "metric": [
            "ContentsCode"
        ]
    },
    "id": [
        "Kjonn",
        "Alder",
        "ContentsCode",
        "Tid"
    ],
    "size": [
        3,
        7,
        1,
        49
    ],
    "dimension": {
        "Kjonn": {
            "label": "gender",
            "category": {
                "index": {
                    "0": 0,
                    "1": 2,
                    "2": 1
                },
                "label": {
                    "0": "Both sexes",
                    "1": "Males",
                    "2": "Females"
                }
            },
            "extension": {
                "elimination": true,
                "show": "value",
                "codeLists": []
            },
            "link": {
                "describedby": [
                    {
                        "extension": {
                            "Kjonn": "urn:ssb:classification:klass:2"
                        }
                    }
                ]
            }
        },
        "Alder": {
            "label": "age",
            "category": {
                "index": {
                    "999B": 0,
                    "00-06": 1,
                    "07-15": 2,
                    "16-44": 3,
                    "45-66": 4,
                    "67-79": 5,
                    "80+": 6
                },
                "label": {
                    "999B": "All",
                    "00-06": "0-6 years",
                    "07-15": "7-15 years",
                    "16-44": "16-44 years",
                    "45-66": "45-66 years",
                    "67-79": "67-79 years",
                    "80+": "80 years or older"
                }
            },
            "extension": {
                "elimination": true,
                "show": "value",
                "codeLists": []
            }
        },
        "ContentsCode": {
            "label": "contents",
            "category": {
                "index": {
                    "Personer": 0
                },
                "label": {
                    "Personer": "Persons"
                },
                "unit": {
                    "Personer": {
                        "base": "number",
                        "decimals": 0
                    }
                }
            },
            "extension": {
                "elimination": false,
                "refperiod": {
                    "Personer": "31.12. until 1990, after that 1.1."
                },
                "show": "value",
                "codeLists": [],
                "measuringType": {
                    "Personer": "Stock"
                },
                "priceType": {
                    "Personer": "NotApplicable"
                },
                "adjustment": {
                    "Personer": "None"
                }
            }
        },
        "Tid": {
            "label": "year",
            "category": {
                "index": {
                    "1845": 0,
                    "1850": 1,
                    "1855": 2,
                    "1860": 3,
                    "1865": 4,
                    "1870": 5,
                    "1875": 6,
                    "1880": 7,
                    "1885": 8,
                    "1890": 9,
                    "1895": 10,
                    "1900": 11,
                    "1905": 12,
                    "1910": 13,
                    "1915": 14,
                    "1920": 15,
                    "1925": 16,
                    "1930": 17,
                    "1935": 18,
                    "1940": 19,
                    "1945": 20,
                    "1950": 21,
                    "1955": 22,
                    "1960": 23,
                    "1965": 24,
                    "1970": 25,
                    "1975": 26,
                    "1980": 27,
                    "1985": 28,
                    "1990": 29,
                    "1995": 30,
                    "2000": 31,
                    "2005": 32,
                    "2010": 33,
                    "2011": 34,
                    "2012": 35,
                    "2013": 36,
                    "2014": 37,
                    "2015": 38,
                    "2016": 39,
                    "2017": 40,
                    "2018": 41,
                    "2019": 42,
                    "2020": 43,
                    "2021": 44,
                    "2022": 45,
                    "2023": 46,
                    "2024": 47,
                    "2025": 48
                },
                "label": {
                    "1845": "1845",
                    "1850": "1850",
                    "1855": "1855",
                    "1860": "1860",
                    "1865": "1865",
                    "1870": "1870",
                    "1875": "1875",
                    "1880": "1880",
                    "1885": "1885",
                    "1890": "1890",
                    "1895": "1895",
                    "1900": "1900",
                    "1905": "1905",
                    "1910": "1910",
                    "1915": "1915",
                    "1920": "1920",
                    "1925": "1925",
                    "1930": "1930",
                    "1935": "1935",
                    "1940": "1940",
                    "1945": "1945",
                    "1950": "1950",
                    "1955": "1955",
                    "1960": "1960",
                    "1965": "1965",
                    "1970": "1970",
                    "1975": "1975",
                    "1980": "1980",
                    "1985": "1985",
                    "1990": "1990",
                    "1995": "1995",
                    "2000": "2000",
                    "2005": "2005",
                    "2010": "2010",
                    "2011": "2011",
                    "2012": "2012",
                    "2013": "2013",
                    "2014": "2014",
                    "2015": "2015",
                    "2016": "2016",
                    "2017": "2017",
                    "2018": "2018",
                    "2019": "2019",
                    "2020": "2020",
                    "2021": "2021",
                    "2022": "2022",
                    "2023": "2023",
                    "2024": "2024",
                    "2025": "2025"
                },
                "note": {
                    "1845": [
                        "The sum of age groups or the sum of sexes can not be added up to the total for the years 1845 and 1855."
                    ],
                    "1855": [
                        "The sum of age groups or the sum of sexes can not be added up to the total for the years 1845 and 1855."
                    ]
                }
            },
            "extension": {
                "elimination": false,
                "show": "code",
                "codeLists": []
            }
        }
    },
    "extension": {
        "px": {
            "infofile": "None",
            "tableid": "05810",
            "decimals": 0,
            "official-statistics": true,
            "aggregallowed": true,
            "copyright": false,
            "language": "en",
            "contents": "05810: Population,",
            "descriptiondefault": false,
            "heading": [
                "ContentsCode",
                "Tid"
            ],
            "stub": [
                "Kjonn",
                "Alder"
            ],
            "matrix": "Personer",
            "subject-code": "be",
            "subject-area": "Population"
        },
        "contact": [
            {
                "name": "Tove Bergseteren",
                "organization": "Statistics Norway",
                "phone": "99 79 08 19",
                "mail": "tov@ssb.no",
                "raw": "Tove Bergseteren, Statistics Norway# +47 99 79 08 19#tov@ssb.no"
            },
            {
                "name": "Magnus Haug",
                "organization": "Statistics Norway",
                "phone": "40 81 14 91",
                "mail": "mgh@ssb.no",
                "raw": "Magnus Haug, Statistics Norway# +47 40 81 14 91#mgh@ssb.no"
            }
        ]
    },
    "value": []
}
```

The metadata consists of a table title `label` (level 1) as well as a list of
variables for the table. For the variable objects dimension (level 2), the most
important properties are:

- Variable id (`id`)
- Variable name (`label`)
- Elimination (`elimination`)

Each variable object contains two lists (level 3), one with value codes index
and one with presentation texts for the values label.

For contents variables, the unit of measurement and the number of decimals are
always specified.

For each value of the content variable, there may be more metadata under extension:

- Measuring type (`Stock`, `Flow`, `Average`, `Other`)
- Price type (`Current`, `Fixed`, `NotApplicable`)
- Seasonal and calender adjustment (`SesOnly`, `WorkOnly`, `WorkAndSes`, `None`)
- Base period

## Retrieve data from table

Data (figures) for a given table number can be retrieved with a URL in the
following endpoint `tables/{TABLE-ID}}/data`.

**Exampel from Statistics Norway for table with `id` *05810* whould look like this:**

> [https://data.ssb.no/api/pxwebapi/v2/tables/05810/data?lang=en](https://data.ssb.no/api/pxwebapi/v2/tables/05810/data?lang=en)

This will provide a table broken down by gender and age for the latest period.
This corresponds to the default extraction you get when you select the same
table in PxWeb. Please note that in the default table, only two dimensions will
show more than one value. To extract numbers in multiple dimensions, this must
be specified in the API query (see section Queries against a table).

!!! note
    The data from Statistics Norway API is in the JSON-stat v2. And that the
    default format that you get the data in may vary depending on the organization.

### Queries against a table

You can use PxWeb to build API queries, or you can create queries from scratch
by looking at the metadata and construct the right objects.

The query contains per variable a list of values that should be included in the
response and also if a specific codelist so be applied.

The query can be represented in two way:

1. As query parameters in the URL when doing a HTTP GET request.
2. As a JSON object attached as the content when doing a HTTP POST request.

#### Queries as URL parameters

Use the lang parameter if you want the English language. The default language is Norwegian. 

To specify variable and values, use valueCodes[variable-id]=value-index1, value-index2 etc. 

You can specify one of several code lists for the variable, use codeList[variable-id]=code-list-id. 

If the code list is a grouping, you can use outputValues[variable-id]=aggregated|single to specify whether you want aggregated or single values. 

Use outputformat if you want to get something other than the default format.

### Truncate with asterisk, mask individual characters with a question mark
In this example from table 03013, we retrieve numbers for the statistic variable (ContentsCode) KpiIndMnd for all consumption groups with a two-digit code (??) and for all months in 2020 (2020*): 

The generated URL looks like this: https://data.ssb.no/api/pxwebapi/v2/tables/03013/data?lang=en&valueCodes[Konsumgrp]=??&valueCodes[ContentsCode]=KpiIndMnd&valueCodes[Tid]=2020*

It is possible to set truncation signs both in front of and behind the search term. 

### Fetch the latest periods, or the first values

If you want figures for the n most recent months, you can use top(n): 

URL: https://data.ssb.no/api/pxwebapi/v2/tables/03013/data?lang=en&valueCodes[Konsumgrp]=??&valueCodes[ContentsCode]=KpiIndMnd&valueCodes[Tid]=top(3) 

With [top(n, start)] you can retrieve n values starting from the start position. Bottom works the same way as top, but retrieves values from the end of the list. 

### Fetch all values from a given starting value
If you want figures for all values from starting value x, you can use from(x):

URL: https://data.ssb.no/api/pxwebapi/v2/tables/03013/data?lang=en&valueCodes[Konsumgrp]=??&valueCodes[ContentsCode]=KpiIndMnd&valueCodes[Tid]=from(2022M01)

### Retrieve all values in a given range
You can use [range (x,y)] to get all values in the range from x to y:

URL: https://data.ssb.no/api/pxwebapi/v2/tables/03013/data?lang=en&valueCodes[Konsumgrp]=[range(01.1,02.2)]&valueCodes[ContentsCode]=KpiIndMnd&valueCodes[Tid]=from(2022M01)

## Output formats

The API can provide the result in 7 main formats:

- `json-stat2`  (default)
- `csv` (text format)
- `px` (used in PxWeb and PxWin)
- `xlsx` (Excel)
- `html`
- `json-px`
- `parquet`

You select the format you want the response to be in by setting the parameter `outputFormat`.

### Additionally paramaters

Some of the output format can take extra parameters that determines how the
table is serialized.

For the formats `csv`, `html` and `xlsx` you can specify the display of
code/text and table title:

- `UseCodes` (display codes)
- `UseTexts` (display text)
- `UseCodesAndTexts` (display codes and text)
- `IncludeTitle` (include table title)

And for `csv` files, you can choose between different separators:

- `SeparatorTab` (tabulator between columns)
- `SparatorSpace` (space between columns)
- `SeparatorSemicolon` (semicolon between columns)

You can set these on the `outputFormatParams` parameter. If multiple parameters
are specified use a comma to separate them apart. E.g.

> <https://data.ssb.no/api/pxwebapi/v2/tables/03024/data?lang=en&valuecodes[ContentsCode]=*&valuecodes[Varegrupper2]=*&stub=VareGrupper2,Tid&heading=ContentsCode&valuecodes[Tid]=top(3)&outputformat=csv&outputformatparams=separatorsemicolon,usecodesandtexts>


TODO note block?
In output formats csv, html, and xlsx, you can use stub to specify which variables to place in the front column of the table and heading for the variables to place in the table header. 

If you place all variables in the stub, you get a so-called pivot-friendly table: https://data.ssb.no/api/pxwebapi/v2/tables/03024/data?lang=en&valuecodes[ContentsCode]=*&valuecodes[Varegrupper2]=*&stub=VareGrupper2,Tid,ContentsCode&valuecodes[Tid]=top(3)&outputformat=csv&outputformatparams=separatorsemicolon,usecodesandtexts 

The decimal separator is . (period) for all languages and all formats, except Excel in Norwegian where the decimal separator is comma. 

## Elimination

An eliminable variable can be completely removed from the query.

If the variable is eliminable (true), it will display:

1. either an elimination value, usually the sum
2. or all values ​​will be aggregated into one.

If the variable is not eliminable (false), you must select something from it.
Time and contents variable (ContentsCode) are not eliminable.

## Valuesets

A variable can have multiple valuesets associated with it. In that case, these
will be listed under endpoint metadata, specified as codeLists/vs. For example,
the region variable can have both a county list and a municipality list:

> <https://data.ssb.no/api/pxwebapi/v2/codeLists/vs_Fylker?lang=en>
> <https://data.ssb.no/api/pxwebapi/v2/codeLists/vs_Kommun?lang=en>

You can specify which value range you want to retrieve numbers from by using
parameter codelist when retrieving data e.g.

> <https://data.ssb.no/api/pxwebapi/v2/tables/01222/data?lang=en&valueCodes[Region]=*&valueCodes[ContentsCode]=Folketallet1&valueCodes[Tid]=from(2022K1)&codelist[Region]=vs_Fylker>

## Groupings (aggregation or selection)

A variable can have groupings associated with it. In that case, groupings will
appear under endpoint metadata, specified as codeLists/agg. An example from

> <https://data.ssb.no/api/pxwebapi/v2/tables/07459/metadata?lang=en>

is Municipalities 2024, aggregated time series:

Excerpt from table *07459* about grouping *agg_KommSummer*
From <https://data.ssb.no/api/pxwebapi/v2/codeLists/agg_KommSummer?lang=en> you
can see how the grouping is constructed:

Excerpt from table *07459* about groupings.
Under `valueMap` you can see which municipality codes are summed up to the group
codes `K-3101`, `K-3203`, etc. The grouping is used together with `outputValues[Region]=`
aggregated to select aggregated numbers, here to sum up numbers for merged
municipalities and municipalities that have changed codes over the years.
Example for *Moss*, which in *2020* was merged by municipalities `0104` and `0136`,
and which in *2024* changed code from `3002` to `3103`:

> <https://data.ssb.no/api/pxwebapi/v2/tables/07459/data?lang=en&valueCodes[Region]=K-3103&valueCodes[Tid]=*&valueCodes[ContentsCode]=Personer1&codelist[Region]=agg_KommSummer&outputValues[Region]=aggregated>

Another example from the same table is Counties 2024-:

Excerpt from table *07459* about grouping *Agg_Fylker2024*
From <https://data.ssb.no/api/pxwebapi/v2/codeLists/agg_Fylker2024?lang=en> you
can see how the grouping is constructed:

Excerpt from table 07459 shows how the grouping is structured.
Here code and valueMap are the same. The grouping is used together with
`outputValues[Region]=` single to select some values ​​from the full list, in
this case the latest county division.  

Example where we retrieve figures for the latest year for all the current county
codes:

> <https://data.ssb.no/api/pxwebapi/v2/tables/07459/data?lang=en&valueCodes[Region]=*&valueCodes[ContentsCode]=Personer1&valueCodes[Tid]=top(1)&codelist[Region]=agg_Fylker2024&outputValues[Region]=single>

## Footnotes

Any footnotes are provided under note and may be associated with a table,
variable, or value. Multiple footnotes may be attached to the same element.
They are then separated by quotation marks and commas. See e.g.
<https://data.ssb.no/api/pxwebapi/v2/tables/12880/metadata?lang=en>.

## POST queries

PxWebApi 2 also supports POST queries. See example at https://github.com/janbrus/ssb-api-python-examples/blob/master/PxWebApi2/laks_nor.ipynb  

## Language

Most of the endpoints take a `lang` parameter specifying 

# What's new in PxWebApi version 2

I have previously worked with SSB's external APIs. I do some work on it in my spare time as a retiree. 
Here is a brief overview of what is new in the new version of PxWebApi v2.

**Most important** is the GET URL support. This makes the API easier to integrate.

**Better masking characters**. You can now mask individual characters with ? , in addition to the existing * for multiple characters. E.g.: 202?M12 - only December numbers for the 2020s.

**New filters**
- from() – retrieve data from and including a starting point
- range([,]) – define a specific interval, e.g. from municipal list
- to() - up to and including (inclusive)
- bottom() - opposite of the existing top()


**Structure control**
- stub – determine which variables should be displayed in the front column
- heading – determine variables in the table header
**Tip**: Place all variables in stub to get a pivot-friendly table


**HTML output** is new in the API.
Styling tips:
```
	<style type="text/css">
	    th[scope="col"] {
	        text-align: center;
	    }
	    th[scope="row"] {
	        text-align: left;
	    }
	    td {
	        text-align: right;
	    }
	    caption{
	    	font-weight: bold;
	    }
	</style>
```

**Known limitations**
**Static URLs**
URLs generated in Statbank are static and do not automatically include future figures. It is necessary to edit it manually to get updated figures. Use e.g. filter from() or top(). In version 1 it was possible to "select all" by eliminating the time variable. This is not possible in v2.

## Response codes

Possible error codes if the query does not return a response:

- 400 – “Bad request” - errors in syntax of the query.
- 403 – Blocking when querying for large data sets. The API limit is 800,000 cells. 
- 404 – Resource not found. May be due to a misspelled URL or URL exceeding the limit of approximately 2100 characters.  
- 429 – Too many queries within a minute. The limit is 30 queries within 60 seconds. Run large queries in sequence. Get the result of the first, before you run the next. 
- 503 – Service unavailable. 

## Known issues

Endpoint navigation has been removed for the time being. Instead, the endpoint tables shows the table's location(s) in the subject structure under paths. An overview of the entire subject structure can be found at https://www.ssb.no/xp/_/service/mimir/subjectStructureStatistics

The URL in the GET request cannot exceed a limit of approximately 2100 characters. Instead of listing long value lists in the query, use * (asterisk), question mark, from/to or range. 

Also note that the GET URL that PxWeb generates will always list exactly the same periods that you selected. In practice, you will most often want the query to include all newer periods the next time you run it. In that case, you must adjust the URL to valueCode[Time]=* or =from(start time), alternatively =top(number of newest periods).

## Other
How to use StatBank 
Before using the API users should be used to the StatBank: How to use StatBank Norway.

It is useful to keep track of the page that provides structural changes and tables that are being terminated and replaced by new ones.

It happens that more values ​​are added to existing code lists. Therefore, you will get a more robust query by using * (asterisk) after valueCode instead of listing all the values.

Special characters in URL
Some special characters are recoded in the URL:

 (space)

%20

+

%2B

"

%22

(

%28

)

%29

[

%5B

]

%5D

If you use Postman to create the query, the characters are changed automatically

Special characters in tables 
The API can display special characters instead of figures: 

. 

Category not applicable 
Figures do not exist at this time, because the category was not in use when the figures were collected. 

.. 

Data not available 
Figures have not been entered into our databases or are too unreliable to be published. 

: 

Confidential 
Figures are not published so as to avoid identifying persons or companies. 

In json-stat2, the special characters are displayed under status, while the data is shown as null. 

JSON 
JSON syntax see: https://www.json.org. 

We recommend having a JSON viewer in your browser. Many browsers have this built-in. You can also download add-ons such as https://jsonview.com.  

JSON-stat 
JSON-stat is a format specifically developed to display statistical tables, that is, datasets with many dimensions. JSON-stat represents the values in the data cubes as a flat array (row-major order). It shows a tree structure, with main elements dataset, dimension, and value with associated status. In addition, contents variables, geographical variables, and time are assigned their own roles ('role') for easy access. 

JSON-stat is used by many statistical agencies, as well as the APIs of Eurostat and the World Bank. There are also ready-made libraries for, among others: Javascript, Python, R, and Java. 

JSON-stat Toolkit is useful, especially for JavaScript. To understand the structure of JSON-stat, it is recommended to try out the JSON-stat explorer. The toolkit also includes JSON-stat Command Line Conversion Tools. These flexible conversion tools such as jsonstat2csv provide a better-customized CSV. This assumes that node.js is installed.

jsonstat2csv - converts JSON-stat into CSV
arrow2jsonstat - converts an Apache Arrow file to JSON-stat
csv2jsonstat - converts CSV into JSON-stat
jsonstat2array - converts JSON-stat into an array of arrays
jsonstat2arrobj - converts JSON-stat into an array of objects
jsonstat2arrow - converts JSON-stat to the Apache Arrow format
jsonstat2objarr - converts JSON-stat into an object of column-oriented arrays
jsonstat2object - converts JSON-stat into a Google DataTable object
jsonstatdice - creates JSON-stat from JSON-stat
sdmx2jsonstat - converts SDMX(JSON) into JSON-stat - convert OECD, UN and IMF API-data to JSON-stat
For JSON-stat examples in Javascript, see: https://observablehq.com/@jsonstat, https://github.com/badosa and https://bl.ocks.org/badosa.

Links to classifications and variable definitions 
In the JSON-stat datasets, we link to a classification like this: {"Kjonn": "urn:ssb:classification:klass:2"}. The number at the end is an ID so that urn:ssb:classification:klass:2 can be rewritten as: https://www.ssb.no/klass/klassifikasjoner/2 or this address in the Klass API: https://data.ssb.no/api/klass/v1/classifications/2?language=en. 

Variable definitions are specified in JSON-stat in the form {CostType: "urn:ssb:conceptvariable:vardok:1116"} where 1116 is an ID. This can be rewritten to https://www.ssb.no/a/metadata/conceptvariable/vardok/1116/en. /en indicates language. The value can be either en, nb, or nn. If you add /xml after /a, you will get XML.

15. Examples of code for using the API 
See https://github.com/janbrus/ssb-api-python-examples/tree/master/PxWebApi2 for simple code examples that use PxWebApi 2. 

16. Contact 
Contact statistikkbanken@ssb.no if you have questions about the tables or the API. It is also possible to provide feedback on GitHub: 

https://github.com/PxTools/PxApiSpecs 

https://github.com/PxTools/PxWebApi  
