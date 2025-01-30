# PX file format Strict
## Readers guide
### What this instruction covers
This manual describes how a PC-Axis PX file is constructed.
The manual is intended for people that wants to get a introduction of how to
use and to extend the Core part of the PC-Axis SDK.
The reader are not required have any type of PC-Axis experience to
understand the content but it helps. 

### How this guide is organized
The document is divided into two part. The first part defines how the PX file
structure works. The second part defined all keywords used in the PC-Axis
File Format.

### Related documents
Earlier versions of the PC-Axis file format.

## Getting started
We must first try to explain the basic structure of the PX-file. PX-files
consist of keywords that describes the contents or the data cube. Some
keywords are mandatory while some are optional.
Some keywords have an language option and some keywords have a sub key
and there is always at least one value specified for a keyword.
Some basic rules for how keywords are written
- The keyword is mandatory.
- Language is optional.
- Language is always written within square brackets.
- Language is always written immediately after the keyword.
- The sub key is optional.
- The sub key is always written within parentheses.
- The sub key is written is written after language if present otherwise it is written after the keyword.
- Value(s) are mandatory.
- Value(s) are always written after the equal sign.
- If there are multiple values each value is written within quotation marks and are separated by a comma sign.
- If there is only one value then it can be written without any quotation marks.
- Values(s) are delimited with quotation marks which means that a value itself cannot contain a quotation mark.
- The end of values (and the beginning of a new keyword) is always
marked with a semicolon.
- Longer texts are divided into several records (except for value text orvariable names which must in one line when in parentheses after a keyword).

```
KEYWORD[LANGUAGE](SUB-KEY)="VALUE_1","VALUE_2", …;
KEYWORD[LANGUAGE](SUB-KEY)=VALUE_1;
KEYWORD[LANGUAGE]="VALUE_1","VALUE_2", …;
KEYWORD (SUB-KEY)="VALUE_1","VALUE_2", …;
KEYWORD[LANGUAGE]=VALUE_1;
KEYWORD(SUB-KEY)=VALUE_1;
```
The file itself is divided into two parts one metadata part and the data part.
The data part starts after the keyword `DATA`.
If the keyword `CHARSET` is missing it means that all texts are in DOS text
format, so that the same files can be used both in the DOS and the Windows
version of PC-Axis.
Starting with version 2005 it is possible to have more than one language in a
PX file. The second language is repeated for all necessary keywords.
`CONTENTS="Population";`, `CONTENTS[sv]=”Befolkning”;` etc. Which
languages are available are given in the keyword `LANGUAGES`.

## Keywords
### Alphabetical list of keywords

|Keyword             |Mandatory|Multiplicity|Language dependent|Default value |Level     |
|--------------------|---------|------------|------------------|--------------|----------|
|AGGREGALLOWED       |No       |0-1         |No                |YES           |TABLE     |
|ATTRIBUTE-ID        |No       |0-1         |No                |              |TABLE     |
|ATTRIBUTE-TEXT      |No       |0-1         |Yes               |              |TABLE     |
|ATTRIBUTES          |No       |0-?         |No                |              |CELLRANGE |
|AXIS-VERSION        |No       |0-1         |No                |              |TABLE     |
|BASEPERIOD          |No       |0-?         |Yes               |              |CONTENT   |
|CELLNOTE            |No       |0-?         |Yes               |              |CELLRANGE |
|CELLNOTEX           |No       |0-?         |Yes               |              |CELLRANGE |
|CFPRICES            |No       |0-?         |No                |              |CONTENT   |
|CHARSET             |No       |0-1         |No                |              |TABLE     |
|CODEPAGE            |No       |0-1         |No                |iso-8859-1    |TABLE     |
|CODES               |Yes      |0-16        |No                |              |VARIABLE  |
|*CONFIDENTIAL       |No       |0-1         |No                |0             |          |
|*CONTACT            |No       |0-?         |Yes               |              |TABLE     |
|*CONTENTS           |Yes      |1           |Yes               |              |TABLE     |
|COPYRIGHT           |No       |0-1         |No                |NO            |TABLE     |
|CREATION-DATE       |Yes      |0-1         |No                |              |TABLE     |
|DATA                |Yes      |1           |No                |              |N/A       |
|*DATANOTE           |No       |0-?         |Yes               |              |TABLE     |
|*DATANOTECELL       |No       |0-1         |Yes               |              |TABLE     |
|DATANOTESUM         |No       |0-1         |Yes               |              |TABLE     |
|DATASYMBOL1         |No       |0-1         |Yes               |.             |TABLE     |
|DATASYMBOL2         |No       |0-1         |Yes               |..            |TABLE     |
|DATASYMBOL3         |No       |0-1         |Yes               |...           |TABLE     |
|DATASYMBOL4         |No       |0-1         |Yes               |....          |TABLE     |
|DATASYMBOL5         |No       |0-1         |Yes               |.....         |TABLE     |
|DATASYMBOL6         |No       |0-1         |Yes               |......        |TABLE     |
|DATASYMBOLNIL       |No       |0-1         |Yes               |-             |TABLE     |
|DATASYMBOLSUM       |No       |0-1         |Yes               |              |TABLE     |
|DAYADJ              |0-1      |0-?         |Yes               |NO            |CONTENT   |
|DECIMALS            |Yes      |1           |No                |              |TABLE     |
|DESCRIPTION         |No       |0-1         |Yes               |              |TABLE     |
|DESCRIPTIONDEFAULT  |No       |0-1         |No                |              |TABLE     |
|DOMAIN              |No       |0-16        |No                |              |VARIABLE  |
|ELIMINATION         |No       |0-16        |Yes               |NO            |VARIABLE  |
|FIRST-PUBLISHED     |No       |0-1         |No                |              |TABLE     |
|HEADING             |Yes      |(0)-1       |Yes               |              |TABLE     |
|HIERARCHIES         |No       |0-1         |Yes               |              |TABLE     |
|HIERARCHYLEVELS     |No       |0-1         |Yes               |              |TABLE     |
|*HIERARCHYLEVELSOPEN|No       |0-1         |Yes               |              |          |
|HIERARCHYNAMES      |No       |0-1         |Yes               |              |TABLE     |
|*INFO               |No       |0-1         |Yes               |              |TABLE     |
|*INFOFILE           |No       |0-1         |Yes               |              |TABLE     |
|*KEYS               |No       |0-16        |Yes               |              |TABLE     |
|LANGUAGE            |Yes      |1           |No                |              |TABLE     |
|LANGUAGES           |No       |0-1         |No                |              |TABLE     |
|LAST-UPDATED        |No       |0-1         |No                |              |TABLE     |
|*LINK               |No       |0-1         |Yes               |              |          |
|MATRIX              |Yes      |1           |No                |              |TABLE     |
|META-ID             |No       |0-?         |Yes               |              |MULTIPLE  |
|NEXT-UPDATE         |No       |0-1         |No                |              |TABLE     |
|NOTE                |No       |0-?         |Yes               |              |MULTIPLE  |
|NOTEX               |No       |0-?         |Yes               |              |MULTIPLE  |
|OFFICIAL-STATISTICS |No       |0-1         |No                |NO            |TABLE     |
|PARTITIONED         |No       |0-16        |Yes               |              |VARIABLE  |
|PRECISION           |No       |0-?         |Yes               |              |CONTENT   |
|PRESTEXT            |No       |0-16        |No                |1             |VARIABLE  |
|REFPERIOD           |No       |0-1/0-?     |Yes               |              |CONTENT   |
|ROUNDING            |No       |0-1         |No                |              |TABLE     |
|SEASADJ             |No       |0-?         |No                |NO            |CONTENT   |
|SHOWDECIMALS        |No       |0-1         |No                |              |TABLE     |
|SOURCE              |No       |0-1         |Yes               |              |TABLE     |
|STOCKFA             |No       |0-?         |Yes               |              |CONTENT   |
|STUB                |Yes      |0-1         |Yes               |              |TABLE     |
|SUBJECT-AREA        |Yes      |1           |Yes               |              |TABLE     |
|SUBJECT-CODE        |Yes      |1           |No                |              |TABLE     |
|SYNONYMS            |No       |0-1         |No                |              |TABLE     |
|*TABLEID            |No       |0-1         |No                |              |TABLE     |
|*TIMEVAL            |No       |0-1         |Yes               |              |TIME      |
|TITLE               |Yes      |  1         |Yes               |              |TABLE     |
|UNITS               |Yes      |1-?         |Yes               |              |CONTENT   |
|UPDATE-FREQUENCY    |No       |0-1         |No                |              |TABLE     |
|VALUENOTE           |No       |0-?         |Yes               |              |VALUE     |
|VALUENOTEX          |No       |0-?         |Yes               |              |VALUE     |
|VALUES              |Yes      |1-16        |Yes               |              |VARIABLE  |
|VARIABLE-TYPE       |No       |0-16        |No                |              |VARIABLE  |
|*DISCONTINUED       |         |            |                  |              |TABLE     |
|*FREQUENCY          |         |            |                  |              |TIME      |

FREQ https://wiki.sdmxcloud.org/SDMX_Time_Formats

```mermaid
---
title: Animal example
---

classDiagram
    Table *--> Variable

    class Table{
        +TITLE
        +MATRIX
    }
    class Variable{
        +CODE
        +TEXT
    }
    class Value{
        +CODE
    }
    class Note{
        +TEXT
    }
```




### Mandatory keywords
These keywords are compulsory. The text (the contents belonging to the key
word) should be written on the same line as the keyword or on a line of its
own, always within quotation marks.
#### CONTENTS
**Type:** Text
**Length:** 256 characters
**Multiline:** No

Information about the contents, which makes up the first part of a title
created when retrieving tables from PC-Axis. The text must not exceed 256
characters (before 2002 only 100 chars).

#### DATA
**Type:** integer/text
**Length:** N/A
**Multiline:** Yes

The keyword `DATA` must be placed at the end of the file, followed by all the
data cells or if `KEYS` are used the variable values and all data cells that differ
from `0`.

##### File without `KEYS`
After the key word follows all data cells that belong to the table.
In most cases the cells are written with one record per line in the table. The
number of records will thus be determined by the number of values in the
stub. The length of the records depends upon the number of values in the
heading, and also upon the size of figures.
Table cells that contain a dash, one, two, three, four, five or six dots should
be within quotation marks.
For data without keys it is possible to write all cells in just one record,
terminated by a semicolon.
PC-Axis accepts the delimiters comma, space, semicolon, tabulator. The
different delimiters are synonyms and can be mixed in the file.
Recommended delimiter is space.

##### Files with `KEYS`
For every variable in the stub is indicated the value for the variable within
quotation marks, comma separated, followed by all data cells for that row
(no quotation marks, space separated).

Whether the text from `VALUES` or from `CODES` are used for a variable is
indicated by the keyword `KEYS(”var”)=CODES` or
`KEYS(”var”)=VALUES`. It is possible to use `VALUES` from one variable
and `CODES` for another in the same file.
Rows that only contain 0 (zeros) are excluded from the file
One - six dots
Data that is missing is stored as one, two, three, four, five or six dots.
It is not possible to get a dot or two dots by excluding a value. Thus, you
cannot write `1,,2,3` in order to get `1,".",3,4`
For PC-Axis version prior to 2005 a dash is presented as 0 and saved as 0.
Thus, you cannot afterwards find out if the original value was - or 0. Starting
with 2005 PC-Axis keeps track on which cells are – and the keyword
`DATASYMBOLNIL` can be used to show something else in the table.
How the dots are shown on screen is determined by the keywords
`DATASYMBOL1`, etc. If these keywords are missing the presentation is
taken from PC-Axis language files.

##### Number of decimals when a file is saved
The keyword `DECIMALS` determines the number of decimals to be saved. If
the file also contains the key word `PRECISION`, PC-AXIS saves as many
decimals as corresponds to the largest number specified by `DECIMALS` or
`PRECISION`. All cells are stored with the same number of decimals.

#### DECIMALS
**Type:** integer [0, 15]
**Length:** Lines of 256 characters
**Multiline:** No

The number of decimals in the table cells. 0 - 15. (0-6 if `SHOWDECIMALS`
is not included). Indicates how many decimals will be saved in the PC-Axis
file. Written without quotation marks. Compare `SHOWDECIMALS`.

#### DESCRIPTION
**Type:** text
**Length:** N/A
**Multiline:** Yes

If a file contains `DESCRIPTION`, when fetching from a disk, this text is used
to show the contents of the disk. C.f. `TITLE`.
If the user wants to save a file in PC-AXIS and writes a text that describes
the file, this text will be saved as `DESCRIPTION`. The text will be used to
show the contents of tables in the SUBJECT AREA. The text is not
presented as a note nor in any other way when the table is presented on the
screen unless the keyword `DESCRIPTIONDEFAULT` is used. In this case
the description is shown instead of the title.

#### HEADING
***Type:*** text
***Length:*** 256 character per value
***Multiline:*** Yes

At least one of the keywords `STUB` or `HEADING` must be included. Usually
both are included, as you choose one or several variables for the stub and the
heading, respectively. The keywords are followed by a list with the chosen
variables. The variables are within quotation marks and separated by
commas. If the list with the variables has to be divided up into several lines,
this should be done after a comma and not within the variable name.

#### MATRIX
**Type:** text
**Length:** 20
**Multiline:** No

The name of the matrix. Is suggested as file name when the file is fetched.
(new length 2008).

#### STUB
**Type:** text
**Length:** 256 characters per value
**Multiline:** Yes

At least one of the keywords `STUB` or `HEADING` must be included. Usually
both are included, as you choose one or several variables for the stub and the
heading, respectively. The keywords are followed by a list with the chosen
variables. The variables are within quotation marks and separated by
commas. If the list with the variables has to be divided up into several lines,
this should be done after a comma and not within the variable name.

#### SUBJECT-AREA
**Type:** text
**Length:** 256 characters
**Multiline:** No

The name of the subject area in plain text, as shown in the menu "Select
subject area". The text must not exceed 256 characters (new length in 2008).

#### SUBJECT-CODE
**Type:** text
**Length:** 20 characters
**Multiline:** No

Subject area code. It is used to create files with tables available in PC-Axis.
The text must not exceed 20 characters (new length 2008).

#### TITLE
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

The title of the table, reflecting its contents and variables. Is not needed if the
file contains `DESCRIPTION`, in that case the text belonging to description is
used. How the heading of a table will look depends on which variables the
user chooses; the heading thus created will then be saved as `TITLE`.
(Compare also `DESCRIPTIONDEFAULT`)
If the file contains `DESCRIPTION` the text for this key word will be used.

#### UNITS
**Type:** text
**Length:** 256 characters
**Multiline:** No

Unit text, e.g. ton, index. The text must not exceed 256 characters (new
length 2008). Compare `UNITS` for `CONTVARIABLE`.
When there is a `CONTVARIABLE` the keyword `UNITS` takes an index and
is repeated for every value for the contents variable. `UNITS("value")="tons"`.

#### VALUES
**Type:** text
**Length:** 256 characters per value
**Multiline:** Yes

The keyword `VALUES` occurs once for each variable in the table, and is
followed by the variable name in parentheses, within quotation marks. The
values will be in the same order as in the stub and heading, respectively.
They are within quotation marks and separated by commas. Each value name
must not exceed 256 characters. If the values have to be divided up into
several lines, this should be done after a comma and not within the value
name. See also the keyword `TIMEVAL` below.

### Optional keywords
#### AGGREGALLOWED
**Type:** {YES,NO}
**Length:** N/A
**Multiline:** No

If the contents of the table cannot be aggregated, contains for instance index
and average, the keyword `AGGREGALLOWED=NO;` is used to stop the
user from making a sum. If the keyword is missing aggregations are allowed

#### ATTRIBUTE-ID
**Type:** text
**Length:** 256 characters per value
**Multiline:** Yes

This keyword list the identities of all attributes. Indirectly it also specifies the
number of attributes.
Each attribute id is separate with a comma sign and it is written within
quotes.
The `ATTRIBUTE-ID` keyword must be written before the `ATTRIBUTES`
keyword and must exist if the `ATTRIBUTES` keyword is present.
The `ATTRIBUTE-ID` keyword is multilingual, that is you do not have to
specify it for each language.

**Example**
```
ATTRIBUTE-ID="ObsStatus","ObsConf";
```

#### ATTRIBUTE-TEXT
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

This keyword specifies a textual representation of the attribute for
presentational purpose and it is optional.
Each attribute text is separate with a comma sign and written within quotes.
The `ATTRIBUTE-TEXT` keyword must occur after the `ATTRIBUTE-ID`
and it is also language dependent. The order of the texts should be the same
as for the order of the ids.

**Example**
```
ATTRIBUTE-TEXT="Observation status","Observation confidence";
ATTRIBUTE-TEXT["sv"]="Status","Tillit";
```

#### ATTRIBUTES
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

This keyword specifies all attributes for as single observation value. Each
attribute value is separated with a comma sign and written within quotes. The
attribute values must appear in the same order as they appeared in the
`ATTRIBUTE-ID` keyword.
The `ATTRIBUTES` keyword must be written after `ATTRIBUTE-ID` and the
`CODES` keyword. Which observation it refers to is given by the key which is
written within parenthesis after the keyword and before the equal sign.
The format of the key is one value code for each variable in the order
indicated by `STUB` and `HEADING` starting with the variables in the `STUB`.
If the attribute key is missing then this will indicate the default values for the
attributes for all measures that do not have any `ATTRIBUTES` specified.
This is also referred to as the default attributes. The default attributes should
always be specified in order to save space and unnecessary repetition.

**Example**
```
ATTRIBUTES("01","A01","2005")="A","P";
ATTRIBUTES="A","F";
```

#### AUTOPEN
**Type:** {YES,NO}
**Length:** N/A
**Multiline:** No
**Default value:** NO

If the file is published on the Internet and the user selects a number of
variables and values it is possible to remove the windows “Select variables
and values” in PC-Axis and instead show the complete table in PC-Axis
when the file is downloaded. `AUTOPEN=YES;`.

#### AXIS-VERSION
**Type:** text
**Length:** 20 characters
**Multiline:** No

Version number for PC-Axis (new text length 2008). Is read and saved but
otherwise not used.

#### BASEPERIOD
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

Base period for, for instance index series. Is shown with the footnote. If there
is a contents variable the keyword is repeated for each value of the contents
variable.

#### CELLNOTE
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

Footnote for a single cell or a group of cells. Which cell it refers to is given
by values and variables. If a value is given as * the note refers to all values
for that variable. Only one value can be given for each variable. The values
are given in the variable order indicated by `STUB` and `HEADING`, starting
with `STUB`.

**Example**
```
CELLNOTE("*","*","Örebro", "1995")="Lekebergs kommun has been excluded from Örebro";
```

#### CELLNOTEX
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

As CELLNOTE but shown mandatory as for NOTEX.

#### CFPRICES
**Type:** {C,F}
**Length:** N/A
**Multiline:** No

Indicates if data is in current or fixed prices. `C` is used for Current and `F` for
Fixed prices. Quotation marks must be used. `CFPRICES="C"` or
`CFPRICES("value")="C"`.

#### CHARSET
**Type:** text
**Length:** 20 characters
**Multiline:** No

`CHARSET=”ANSI”;` indicates that the texts in the file are written in
Windows format. If the keyword is missing it means that the texts in the file
are in DOS format. They will be translated by PC-Axis to Windows. This
keyword must appear in the beginning of the file before any texts that can
include characters outside A-Z, 0-9.

#### CODEPAGE
**Type:** text
**Length:** 20 characters
**Multiline:** No

Is used when creating XML format to get correct characters. Default `iso8859-1`. Max 20 chars.

#### CODES
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

The key word `CODES` is used if a variable exists both in code and plain text.
The codes are written in the same way as `VALUES`. Not more than 256
characters.

#### CONFIDENTIAL
**Type:** integer
**Length:** 20 characters
**Multiline:** No
**Default value:** 0

Possibility to do some manipulation with the data in the data part of the file.
Is only suitable if the user cannot download the total file since the data can
be read in any editor. Max 20 chars.
The only other value that it can take is 1 and that uses the simple disclosure
control. Which is that it displays values of 1 to be 0 and values of 2 to be 3.

#### CONTACT
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

States the person who can give information about the statistics. Is written in
the form name, organization, telephone, fax, e-mail. Several persons can be
stated in the same text string and are then divided by the #-sign. Is shown
with the footnote. If there is a contents variable the keyword is repeated for
each value `CONTACT("value")="xx"`.

**Example**
```
CONTACT="Maria Svensson, SCB, +4619176800, +4619176900,
maria.svensson@scb.se";
```

#### CONTVARIABLE
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This is used to indicate that the table has two or more different contents. For
instance the contents Import and the contents Export. The variable name
must also be found either as `STUB` or `HEADING`. When a content variable
exists a number of keywords will be indexed: `DAYADJ`, `SEASADJ`,
`STOCKFA`, `UNITS`, `CONTACT`, `LASTUPDATED`, `REFPERIOD`,
`BASEPERIOD`, `CFPRICES`. The keyword `CONTVARIABLE` must proceed
the first keyword that will be indexed.

#### COPYRIGHT
**Type:** text
**Length:** 20 characters
**Multiline:** No

Copyright is given as `YES` or `NO`. If `COPYRIGHT=YES` the copyright
refers to the organization given in `SOURCE`. Is shown together with
footnotes.

#### CREATION-DATE
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

Date when file was created. Written in format `CCYYMMDD hh:mm`, e.g.
`”19960612 14:20”`. Is shown together with footnotes.

#### DATABASE
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

The name of the database from where the statistics is retrieved. Is shown
with the footnote.

#### DATANOTE
**Type:** text
**Length:** 20 characters
**Multiline:** Yes

The `DATANOTE` keyword is used to indicate that a note exists for a certain
element of the statistical cube
If no sub key is specified like below the data note * is valid for the hole
table.
`DATANOTE="*";`
If there is just one part sub key like below. Then the sub key must refer to a
variable and the data note is for that variable.
`DATANOTE("VARIABLE")="*";`
The example above states that the data note * should be displayed for
variable `VARIABLE`.
If the sub key is divided into two parts. Then the first part of the sub key
refers to a variable and the second part refers to a value for that variable.
`DATANOTE("VARIABLE","VALUE")="*";`
The example above states that the data note * should be displayed for value
`VALUE` for variable `VARIABLE`.
The keyword is multilingual so different data notes can be used for different
languages.
It is recommended to add the data note to the note text. E.g. if you have a
note on the time period 2010 it could look something like this in the PX file.

```
DATANOTE("tid","2010")="*";
VALUENOTE("tid","2010")="* Preliminary results";
```

#### DATANOTECELL
**Type:** text
**Length:** 20 characters
**Multiline:** No
This keyword needs information for which value for each variable it applies
and the symbol is presented together with the figure. CODES are used in the
parentheses to indicate the right value. * means all values for the variable.

**Example**
```
DATANOTECELL("*", "20", "*", "BE0101F2", "*")="Ae";
```

#### DATANOTESUM
**Type:** text
**Length:** 20 characters
**Multiline:** No

This keyword gives information which symbol is presented together with the
figure after a sum has been made consisting of differing datanote symbols.

#### DATASYMBOL1, DATASYMBOL2, DATASYMBOL3, DATASYMBOL4, DATASYMBOL5, DATASYMBOL6
**Type:** text
**Length:** 20 characters
**Multiline:** No

This if used to indicate how a stored “.” is to be presented in a table.

#### DATASYMBOLNIL
**Type:** text
**Length:** 20 characters
**Multiline:** No

This is used to indicate how a stored “-” is to be presented in a table. The `–`
indicates that the figure is absolutely nil.

#### DATASYMBOLSUM
**Type:** text
**Length:** 20 characters
**Multiline:** No

This if used to indicate how a sum of differing numbers of dots will be
shown. The sum is stored as `“…….”`.

#### DAYADJ
**Type:** {YES, NO}
**Length:** N/A
**Multiline:** No

`DAYADJ=YES` means that data is adjusted e.g. to take into account the
number of working days. Default is `DAYADJ=NO` or
`DAYADJ("value")=NO`.

#### DEFAULT-GRAPH
**Type:** integer
**Length:** 1-10
**Multiline:** No

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### DESCRIPTIONDEFAULT
**Type:** {YES, NO}
**Length:** N/A
**Multiline:** No

For some languages it is difficult to build a table title dynamically. The
keyword `DESCRIPTIONDEFAULT=YES;` means that the text after
keyword Description will be used as title for the table.

#### DIRECTORY-PATH
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### DOMAIN
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

Can occur once for each variable. Is used to determine which value sets are
of interest, and thus which aggregation lists can be used. (New text length
2008).

#### DOUBLECOLUMN
**Type:** {YES, NO}
**Length:** N/A
**Multiline:** No

This keyword is used to get code and text in separate columns for the
specified variable `DOUBLECOLUMN(”region”)=YES;`. On screen it only
has effect if the user selects presentation in matrix format.

#### ELIMINATION
**Type:** {YES, NO, text}
**Length:** 256 characters
**Multiline:** Yes

States if and how a variable may be eliminated in a table. If the key word is
written as `ELIMINATION("variable name")="value name"` this value will be
used as an elimination value if the user does not select the variable to the
table. If the key word is written `ELIMINATION("variable name")=YES` this
means that the variable will be eliminated by the summing up of all the
values for that variable in the file.

#### FIRST-PUBLISHED
**Type:** {YES, NO, text}
**Length:** 256 characters
**Multiline:** Yes

The date when the data cube was first published in the format `CCYYMMDD hh:mm`.

**Example**
```
FIRST-PUBLISHED=20130224 20:55
```

#### HIERARCHIES
**Type:** text
**Length:** Lines of 256 characters for each value
**Multiline:** Yes

**Example 1**
```
HIERARCHIES(“Country”)="parent","parent":"child",...
```

**Example 2**
```
HIERARCHIES("Country")=”E25","E25":"E15","E15":"E12",
"E12":"AT","E12":"BE","E12":"FI","E12":"FR","E12":"DE",
"E12":"GR","E12":"IR","E12":"IT","E12":"LU","E12":"NL",
"E12":"PT","E12":"ES","E15":"DK","E15":"SW","E15":"UK",
"E25":"E10","E10":"CY","E10":"CZ","E10":"EE","E10":"HU",
"E10":"LV","E10":"LT","E10":"MT","E10":"PL","E10":"SK", "E10":"SI";
```

#### HIERARCHYLEVELS
**Type:** integer
**Length:** N/A
**Multiline:** No

Indicate the number of levels existing for a symmetrical tree.

**Example**
```
HIERARCHYLEVELS("Country")=4
```

#### HIERARCHYLEVELSOPEN
**Type:** integer
**Length:** N/A
**Multiline:** No

Indicate how the tree is to be displayed when the tree is shown for the first
time

**Example**
```
HIERARCHYLEVELSOPEN("Country")=1
```
the first level is open.

#### HIERARCHYNAMES
**Type:** text
**Length:** Lines of 256 characters per name
**Multiline:** Yes

To give names to the levels in a symmetrical tree.

**Example**
```
HIERARCHYNAMES("Country")="NameOfLevel1","NameOfLevel2",..
```

#### INFO
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### INFOFILE
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

Name of a file containing more information for the statistics. If the keyword
exists in the file a new button is shown in the toolbar and the user can click
for more information. Depending on the file type and to which program the
type is associated the corresponding program is started and the information
shown. (Word for .DOC, Notepad for .TXT etc).

#### KEYS
**Type:** {VALUES, CODES}
**Length:** N/A
**Multiline:** No

If this keyword is used it must occur as many times as there are variables in
the stub. It contains the name of the variable and whether the key is taken
from `VALUES` or `CODES`. 

**Example**
```
KEYS("age")=VALUES;
KEYS("region")=CODES;
```
When it is used all data rows start with the Value/code of the variables in the
stub. See Description of `DATA`.

#### LANGUAGE
**Type:** text
**Length:** 2 characters
**Multiline:** No
The language used in the PC-Axis file (2 chars), sv for Swedish, en for
English etc. Compare language codes for text files. If the keyword is used,
the words for “and” and “by” are read from the text file of that language.
Otherwise these words are read from the text file of the language in which
the program is running.

#### LANGUAGES
**Type:** text
**Length:** 2 characters per language
**Multiline:** Yes

If more than one language in the PC-Axis file then all languages are
mentioned here (2 chars each)

**Example**
```
LANGUAGES="en","sv";
```

#### LAST-UPDATED
**Type:** text
**Length:** 256 characters
**Multiline:** No

Date and time for latest update format `CCYYMMDD hh:mm`. Example
`”19960528 11:35”`. Is also used in Aremos file format. The date is not
updated in PC-AXIS when changes are made to the table. If there is a
contents variable it is written as `LAST-UPDATED("value")= 19990318 18:12`

#### LINK
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### MAP
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

Used for a geographic variable for which maps can be made (new text length
2008). 

**Example**
```
MAP("region")="Sweden_municipality";
```

#### META-ID
**Type:** text
**Length:** 256
**Multiline:** No

The `META-ID` keyword is used to reference a external meta information
about a table, variable or value.
I there is no subkey specified the `META-ID` refers to the cube
`META-ID="RT:12";`
If there is just one part sub key like below. Then the sub key must refer to a
variable and the data note is for that variable.
`META-ID("VARIABLE")="V:167";`
The example above states that the reference id for variable `VARIABLE` is
`V:167`.
If the sub key is divided into two parts. Then the first part of the sub key
refers to a variable and the second part refers to a value for that variable.
`META-ID("VARIABLE","VALUE")=VAL:232;`
The example above states that the reference id for value `VALUE` for variable
`VARIABLE` is `VAL:232`.

#### NEXT-UPDATE
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### NOTE
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

Contains a footnote which is showed in the statistical databases if the user
demands it. In PC-AXIS it is shown if the user presses F7. The footnote may
either refer to the entire table or to a table variable. In the latter case the key
word must be followed by the variable name in parentheses.

#### NOTEX
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

Contains a note which is always shown in the statistical databases. In PCAXIS the note is shown automatically, before the table is presented on the
screen. The note may either refer to the entire table or to a table variable. In
the latter case the key word should be followed by the variable name in
parentheses.

#### OFFICIAL-STATISTICS
**Type:** {YES, NO}
**Length:** N/A
**Multiline:** No

Indicates if the data table is included in the official statistics of the
organization.

#### PARTITIONED
**Type:** complex text
**Length:** N/A
**Multiline:** Yes
This is used to partition a variable into levels, for instance
```
PARTITIONED("region")="municipality",1,4;
PARTITIONED("region")="subarea",5;
```
states that the first four positions for the regional values contain the
municipality code, and that the subarea code starts in position 5. Thus the
values for the variable region after the key word `VALUES` must be written in
code, not plain text. Max 3 levels can be used, each gives start position and
length except for the last level where length is implied as rest of the code.

#### PRECISION
**Type:** integer
**Length:** N/A
**Multiline:** No
Can occur for single values. Determines that the value shall be presented
with a number of decimals that differs from the keyword
`SHOWDECIMALS`. Is to be written as
`PRECISION("variable name","value name")=n` where n is a figure between `1`
and `6`. The number of decimals for precision must be higher than the number
of decimals for `SHOWDECIMALS` to have any effect.

#### PRESTEXT
**Type:** integer
**Length:** N/A
**Multiline:** No

From PX file format version 2010 and later the definition of PRESTEXT is
changed the prior versions defines it as:
States if texts or codes are shown for the keyword `VALUES`. Normally a file
is created so that texts are found after the keyword `VALUES` and codes after
the keyword `CODES`. This is equivalent to `PRESTEXT("variable name")=1;`
which is the default. If a user changes presentation from texts to codes and
saves the file the value codes will be saved after the keyword `VALUES` and
value texts after the keyword `CODES`. In this case the keyword `PRESTEXT` is
written as `PRESTEXT("variable name")=0`. This enables PC-Axis to know if
it is necessary to switch to codes if aggregation is selected. The user can also
decide to show both codes and texts for a value and in this case the keyword
is saved as `PRESTEXT("variable name")=2` or `PRESTEXT("variable name")=3`. `PRESTEXT` becomes 2 if it originally was 1 and 3 if it originally
was 0.
In the new definition the `PRESTEXT` is that it is an integer value between 0-
3 that describes how the value should be presented in a user interface. The
values text should always be defined by the `VALUES` keyword and the
codes should always be defined by the `CODES` keyword.
- 0 - Only the value code should be displayed.
- 1 - Only the value text should be displayed.
- 2 - Both code and value should be displayed and the order should be
the code and then the value text.
- 3 - Both code and value should be displayed and the order should be
the value text then the value code.

**Example**
```
VALUES("region")="Albania","Austria",…
…
CODES("region")="AL","AT",…
…
PRESTEXT("region")=2
Since the PRESTEXT is set to 2 the values would be presented as
AL Albania
AT Austria
…
```

#### PX-SERVER
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### REFPERIOD
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

Text with information on the exact period for the statistics. Is shown with the
footnote. If there is a contents variable the keyword is repeated for each
value of the contents variable.

#### ROUNDING
**Type:** integer
**Length:** 0-1
**Multiline:** Yes

If rounding is missing then rounding will be done in accordance with
information in the pcax2000.ini file. If the keyword is missing then the
decimal rule in pcax2000.ini is used.
- Rounding = `0` means that .5 is rounded up if the previous figure is odd, 1.5
becomes 2.
- Rounding = `1` means that .5 is always rounded up. 0.5 becomes 1 and 1.5
becomes 2.

#### SEASADJ
**Type:** {YES, NO}
**Length:** N/A
**Multiline:** No

`SEASADJ=YES` means that data is seasonally adjusted. Default is
`SEASADJ=NO` or `SEASADJ("value")=NO`.

#### SHOWDECIMALS
**Type:** integer
**Length:** N/A
**Multiline:** No

The number of decimals to be shown in the table, 0-6. Must be the same or
smaller than the number stored as indicated by the keyword `DECIMALS`. If
`SHOWDECIMALS` is not stated in the file the number stated by
`DECIMALS` will be used.

#### SOURCE
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

States the organization which is responsible for the statistics. Is shown with
the footnote.

#### STOCKFA
**Type:** {S, F, A}
**Length:** N/A
**Multiline:** No

Indicates if data is stock, flow or average. The used characters `S` (stock), `F`
(flow) and `A` (average) must be within quotation marks. `STOCKFA="S"` or
`STOCKFA("value")="S"`.

#### SURVEY
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis. Is
shown on information screen in PX-web if installation parameter true.

#### SYNONYMS
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This is used for search in PX-Web. If the table concerns CPI the synonym
can be “Inflation”. Several words can be included within “ “ and the total line
can be max 256 chars.

#### TABLEID
**Type:** text
**Length:** 256 characters
**Multiline:** Yes
A text that is the identity of the table.

#### TIMEVAL
**Type:** text
**Length:** N/A
**Multiline:** Yes

New keyword in version 1.5 to enable the use of time series. After the
keyword is the name of the time variable given, e.g. `TIMEVAL(”time”)`.
`TLIST` gives information on timescale and timeperiods. The time periods
must be consecutive.
If the table contains the time periods `1994`, `1995`, `1996` `TIMEVAL` can be
written in this way:
```
TIMEVAL(”time”)=TLIST(A1, ”1994”-”1996”);
```
or
```
TIMEVAL(”time”)=TLIST(A1), ”1994”, ”1995”,"1996”;
```
The following formats are used:
- `A1` for annual statistics written as `CCYY` (`C` for century, `Y` for year)
- `H1` for halfyear in format `CCYYH`, where `H` is 1 or 2
- `Q1` for quarterly data written `CCYYQ`, where `Q` is 1 - 4.
- `M1` for monthly statistics written `CCYYMM`
- `W1` for weekly data written `CCYYWW`

`TIMEVAL` is used in PC-AXIS version 1.7 when converting to Aremos and
Gesmes/Ecoser format. It is also used for start and stop time in list of
contents in HTML-format.
The variable time can be used both after the keyword `VALUES` and after
`TIMEVAL`. If both exist `TIMEVAL` must be placed after `VALUES`. If only
`TIMEVAL` exists `VALUES` for time are created as follows:
Time scale time periods shown as
```
A1 1995, 1996, 1997
H1 1995H1, 1995H2, 1996H1
Q1 1995Q1, 1995Q2, 1995Q3
M1 1995M01, 1995M02, 1995M03
W1 1995W01, 1995W02, 1005W03
```

#### UPDATE-FREQUENCY
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-axis.

#### VALUENOTE
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

A footnote for separate variable values. Should be written with the variable
name and the value names in parentheses.

#### VALUENOTEX
**Type:** text
**Length:** Lines of 256 characters
**Multiline:** Yes

Mandatory footnote for single values for a variable. Is written with the
variable name and the value name in parentheses. Is shown the same way as
`NOTEX`.

#### VARIABLE-TYPE
**Type:** text
**Length:** 256 characters
**Multiline:** Yes

This keyword is read and saved in the PX-file but not shown in PC-Axis.

### Recommended order of the keywords
Some keywords must be written before some other keywords e.g. `STUB` and
`HEADING` must precede `VALUES` and `CODES`. Here follows a list of
recommended order for how keywords should be written I PX-files though
applications should not depend on that this order is used.

- CHARSET
- AXIS-VERSION
- CODEPAGE
- LANGUAGE
- LANGUAGES
- CREATION-DATE
- NEXT-UPDATE
- PX-SERVER
- DIRECTORY-PATH
- UPDATE-FREQUENCY
- TABLEID
- SYNONYMS
- DEFAULT-GRAPH
- DECIMALS
- SHOWDECIMALS
- ROUNDING
- MATRIX
- AGGREGALLOWED
- AUTOPEN
- SUBJECT
- CODE
- SUBJECT-AREA
- CONFIDENTIAL
- COPYRIGHT
- DESCRIPTION
- TITLE
- DESCRIPTIONDEFAULT
- CONTENTS
- UNITS
- STUB
- HEADING
- CONTVARIABLE
- VALUES
- TIMEVAL
- CODES
- DOUBLECOLUMN
- PRESTEXT
- DOMAIN
- VARIABLE-TYPE
- HIERARCHIES
- HIERARCHYLEVELS
- HIERARCHYLEVELSOPEN
- HIERARCHYNAMES
- MAP
- PARTITIONED
- ELIMINATION
- PRECISION
- LAST-UPDATED
- STOCKFA
- CFPRICES
- DAYADJ
- SEASADJ
- UNITS
- CONTACT
- REFPERIOD
- BASEPERIOD
- DATABASE
- SOURCE
- SURVEY
- LINK
- INFOFILE
- FIRST-PUBLISHED
- META-ID
- OFFICIAL-STATISTICS
- INFO
- NOTEX
- NOTE
- VALUENOTEX
- VALUENOTE
- CELLNOTEX
- CELLNOTE
- DATASYMBOL1
- DATASYMBOL2
- DATASYMBOL3
- DATASYMBOL4
- DATASYMBOL5
- DATASYMBOL6
- DATASYMBOLSUM
- DATASYMBOLNIL
- DATANOTECELL
- DATANOTESUM
- DATANOTE
- KEYS
- ATTRIBUTE-ID
- ATTRIBUTE-TEXT
- ATTRIBUTES
- PRECISION
- DATA
