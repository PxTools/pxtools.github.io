# PxWebApi 2 user guide

This guide describes how the PxWebApi works in general. Note that each
organization that installs PxWebApi to make their database accessible can
configure the API to suit their own needs by setting different limits, licenses,
languages, or rate controls according to their own policies.

!!! note "Examples"
    Examples in this guide will be done against Statistic Norways API for their
    Statbank.

See also the [OpenAPI specifications for the PxWebApi 2](https://github.com/PxTools/PxApiSpecs)
at GitHub.

## What's new in PxWebApi version 2

Some of the most important changes in version 2 of the PxWebApi are:

- **Most important** is the GET URL support. This makes the API easier to integrate.
- **Better masking characters**. You can now mask individual characters with `?`,
  in addition to the existing `*` for multiple characters. E.g.: `202?M12`
  selects only December numbers for the 2020s.
- **New selection expressions**
    - `from` retrieve data from and including a starting point.
    - `range` define a specific interval, e.g. from municipal list.
    - `to` up to and including (inclusive).
    - `bottom` opposite of the existing `top`.
- **Structure control**
    - `stub` – determine which variables should be displayed in the front column
    - `heading` – determine variables in the table header

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
get the tables in english for Statistics Norways database with the following URL

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

The response will contains a list of tables that are in the database in that
specific language. It also conatins information about result pages. The hole list
is not returned in one response instead the list is split up in pages containing
subparts of the complete list of tables.

The `tables` endpoint can take the following optional arguments to filter and
select a specific slice of the resulting list of tables:

- `lang` an language parameter specifying which language the result should be in.
- `query` an query string that can filter out tables.
- `pastDays` filters out tables in the result so only tables updated since the
   number of `pastDays`.
- `includeDiscontinued` if discontinued tables should be included or not in the result.
- `pageSize` All tables might not be included in the response depending on how
   many tables there is in the result and the `pageSize` sets how many tables
   are returned in the response. The default value might be changed by the organization.
- `pageNumber`  The `pageNumber` specifies which page should be in the response.
  The default is the first page.

### Examples

#### Search for "pizza" in all tables

Add the `query` parameter to the URL like

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

> <https://data.ssb.no/api/pxwebapi/v2/tables/05810?lang=en>

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
- Seasonal and calendar adjustment (`SesOnly`, `WorkOnly`, `WorkAndSes`, `None`)
- Base period

## Retrieve data from table

Data (figures) for a given table number can be retrieved with a URL in the
following endpoint `tables/{TABLE-ID}}/data`.

**Example from Statistics Norway for table with `id` *05810* whould look like this:**

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

When the listing the values that should be selected you can either list the
specific codes or you could use special selection expressions to select value
codes that matches the expression. You can also combine value codes and.
selection expressions.

#### Query as URL parameters

To specify variable and values, use the following syntax:

```
valueCodes[variable-id]=value-code1, value-code2
```

You need to specify value codes for each variable that is not eliminable. The
value codes are given in a comma separated list. If the value code should be
surrounded by square brackets like `3,[1,2],4+5`. See bellow on how to specify
value codes.

You change can specify a different codelist for a variable the possible once
are given in the metadata response. In these cases, you need to add the paramter
`codelist` that specifies which codelist to use. When doing so the value codes referred
in `valueCodes` are the value codes in the selected codelist.
You select the codelist in the following syntax

```
codelist[variable-id]=codelist-id.
```

You can also specify the format you want the data in. See Output formats bellow.

#### Query in the POST body

You can also specify the query as an JSON object and pass that in the body of a
HTTP POST request.

The syntax of the body is as follows:

```json
{
  "selection": [
    {
      "variableCode": "variable id",
      "codelist": "",
      "valueCodes": [
        "value-code1",
        "value-code2",
        "etc"
      ]
    }
  ],
  "placement": {
    "heading": [
      "string"
    ],
    "stub": [
      "string"
    ]
  }
}
```

You need to provide a selection object in the `selection` array for each
variable that is not eliminable. Each selection object can contain the
following properties:

- `variableCode` the id of the variable
- `valueCodes` an array of value codes that should be selected for
  that specific variable.
- `codelist` (optional) if a specific codelist should be selected for that variable.

??? tip "Doing POST queries in Python examples"
    See how you can do POST queries in Python here <https://github.com/janbrus/ssb-api-python-examples/blob/master/PxWebApi2/laks_nor.ipynb>

#### Selection expressions

Selection expression  can be used to select value codes that matches the expression.
The following expressions exists:

- `*` (wildcard)
- `?` (mask)
- `top`
- `bottom`
- `range`
- `to`
- `from`

##### `*` (wildcard)

This matche based on a criteria that contains 1 or 2 wildcards e.g.

- `*` selects all codes.
- `12*` select all codes that starts with *12*.
- `*2` selects all codes that ends with *2*.
- `*4*` select all codes that contains a *4*.

##### `?` (mask)

This matche on a criteria that contains a question mark e.g.

- `??` selects all codes that are two characters long.
- `1?` select all codes that are two characters long and starts with `1`.

##### top

This expression selects the top number of values. If the variable is the time
variable that will be the latest time periods otherwise it will be the first code
as specified in the metadata.

The syntax of the experssion is:

```
top(numberOfValues, offset)
```

where `numberOfValues` specifies the number of values codes that should be
selected from the top and `offset` is an optional offset from the top. E.g.

- `top(5)` will select the first 5 values.
- `top(5, 1)` will skip the first value and select the next 5 values.

##### bottom

This expression selects the bottom number of values. If the variable is the time
variable that will be the first time periods otherwise it will be the last code
as specified in the metadata.

The syntax of the experssion is:

```
bottom(numberOfValues, offset)
```

where `numberOfValues` specifies the number of values codes that should be
selected from the bottom and `offset` is an optional offset from the bottom. E.g.

- `bottom(5)` will select the last 5 values.
- `bottom(5, 1)` will skip the last value and select the next 5 values counting
  from the bottom.

##### range

This expression selects all value code between two value codes as they are given
in the metadata.

The syntax is in the form

```
range(value-code1, value-code2)
```

Example if you have a time variable that have codes from the year 2000 to 2025
then `range(2002,2005)` would select the codes for the years 2002 to 2005.

##### from

This expression selects all value code from the specified value code.

The syntax is in the form

```
from(value-code1)
```

Example if you have a time variable that have codes from the year 2000 to 2025
then `from(2005)` would select the codes for the 2005 to 2025. When new data
comes for the year 2026 that will also be included.

##### to

This expression selects all value code the bottom to the specified value code.

The syntax is in the form

```
to(value-code1)
```

Example if you have a time variable that have codes from the year 2000 to 2025
then `to(2005)` would select the codes from the 2000 to 2005.

#### Variable placements

The placement of variables can be set when fetching data. When doing `GET` query
you set the `stub` and `heading` query parameters and specify the variables id
comma separated from each other.

When doing a POST query, the placement are given i the `placement` property of
the query.

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

??? info "About JSON-stat v2"
    JSON-stat is a format specifically developed to display statistical tables,
    that is, datasets with many dimensions. JSON-stat represents the values in
    the data cubes as a flat array (row-major order). It shows a tree structure,
    with main elements dataset, dimension, and value with associated status. In
    addition, contents variables, geographical variables, and time are assigned
    their own roles ('role') for easy access.

    JSON-stat is used by many statistical agencies, as well as the APIs of 
    Eurostat and the World Bank. There are also ready-made libraries for, among 
    others: Javascript, Python, R, and Java. 

    JSON-stat Toolkit is useful, especially for JavaScript. To understand the 
    structure of JSON-stat, it is recommended to try out the JSON-stat explorer.
    The toolkit also includes JSON-stat Command Line Conversion Tools. These
    flexible conversion tools such as jsonstat2csv provide a better-customized
    CSV. This assumes that node.js is installed.

    - `jsonstat2csv` converts JSON-stat into CSV
    - `arrow2jsonstat` converts an Apache Arrow file to JSON-stat
    - `csv2jsonstat` converts CSV into JSON-stat
    - `jsonstat2array` converts JSON-stat into an array of arrays
    - `jsonstat2arrobj` converts JSON-stat into an array of objects
    - `jsonstat2arrow` converts JSON-stat to the Apache Arrow format
    - `jsonstat2objarr` converts JSON-stat into an object of column-oriented arrays
    - `jsonstat2object` converts JSON-stat into a Google DataTable object
    - `jsonstatdice` creates JSON-stat from JSON-stat
    - `sdmx2jsonstat` converts SDMX(JSON) into JSON-stat - convert OECD, UN and 
       IMF API-data to JSON-stat

    For JSON-stat examples in Javascript, see:

    -  <https://observablehq.com/@jsonstat>
    -  <https://github.com/badosa>
    -  <https://bl.ocks.org/badosa>.

### Additionally parameters

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

!!! tip "Placement of variables"
    In output formats `csv`, `html`, and `xlsx`, you can use `stub` and `heading`
    to specify where variables are to be placed.
    If you place all variables in the `stub`, you get a so-called pivot-friendly
    e.g. <https://data.ssb.no/api/pxwebapi/v2/tables/03024/data?lang=en&valuecodes[ContentsCode]=*&valuecodes[Varegrupper2]=*&stub=VareGrupper2,Tid,ContentsCode&valuecodes[Tid]=top(3)&outputformat=csv&outputformatparams=separatorsemicolon,usecodesandtexts>

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

> <https://data.ssb.no/api/pxwebapi/v2/tables/12880/metadata?lang=en>.

## Language

Most of the endpoints take a `lang` parameter specifying which language the response
should be in.

## Response codes

Possible error codes if the query does not return a response:

- `400` – “Bad request” - errors in syntax of the query.
- `403` – Blocking when querying for large data sets. The API limit varies between
  organizations cells.
- `404` – Resource not found. May be due to a misspelled URL or URL exceeding
  the limit of approximately 2100 characters.
- `429` – Too many queries within a minute. The limit varies between organizations.
  Run large queries in sequence. Get the result of the first, before you run the
  next.
- `503` – Service unavailable.

## Known issues

- Endpoint navigation has been removed for the time being. Instead, the endpoint
  tables shows the table's location(s) in the subject structure under paths.

- The URL in the GET request cannot exceed a limit of approximately 2100
  characters. Instead of listing long value lists in the query, use * (asterisk),
  question mark, from/to or range.

- Also note that the GET URL that PxWeb generates will always list exactly the
  same periods that you selected. In practice, you will most often want the query
  to include all newer periods the next time you run it. In that case, you must
  adjust the URL to `valueCode[Time]=*` or `from(start time)`, alternatively
  `top(number of newest periods)`.
