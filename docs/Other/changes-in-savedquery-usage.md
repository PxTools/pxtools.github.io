# Changes in savedquery usage

## Saved queries in PxWeb2 and PxWebApi2

A saved query has an output format, which was selected when it was created.

If the output format is "Screen" you should get the table with your selections
in the gui. If the output is "Download" (e.g. excel) the browser should download
the data in the chosen format.

In the old classic version, the url of the gui was used for both "screen" and
"download", and the output format "screen"  got you pxweb "as it was" when the
query was created. If the output format was excel, you would not see the gui,
the browser would just start to download a file.

In the new version, the responsibillity is sharded between the api and and gui:
The url to the gui is used to get pxweb "as it was", also for "download" output
formats. The url to the api is used to download  the data of any savedquery.
For the "sceen" output format will download jsonstat2 files.

`http://mysite/api/pxwebapi/v2/savedqueries/N/data`  will download the file.

`http://mysite/api/pxwebapi/v2/savedqueries/N` will get you the definition of
the savedquery.

## Saved Query vs. API Query ##

Some have used saved query created in PxWeb version 1 as a source in Power Query (Excel and Power BI). PxWeb version 2 does not support formats such as csv or xlsx from saved queries, only display on screen. In PxWeb v2 saved query is called “Link to updated table”.

For example, the saved query `https://statistikdatabasen2.scb.se/sq/502699` will no longer generate an xlsx file in the new interface. Power Query will return an error message.

You can see how the query is defined by entering: https://statistikdatabasen.scb.se/api/v2/savedqueries/132129

Format: `https://statistikdatabasen.scb.se/api/v2/savedqueries/(number of saved query)`

You can download the file using: [https://statistikdatabasen.scb.se/api/v2/savedqueries/502699/data&lang=en](https://statistikdatabasen.scb.se/api/v2/savedqueries/132129/data?lang=en)

Format: `https://statistikdatabasen.scb.se/api/v2/savedqueries/(number of saved query)/data`

The variables and values selected in the query are shown at: [https://statistikdatabasen.scb.se/api/v2/savedqueries/502699/selection&lang=en](https://statistikdatabasen.scb.se/api/v2/savedqueries/132129/selection?lang=en)

Format: `https://statistikdatabasen.scb.se/api/v2/savedqueries/(number of saved query)/selection?lang=en`

In addition, it is possible to specify output formats and output parameters, for example like this:
[https://statistikdatabasen.scb.se/api/v2/savedqueries/502699/data?outputFormat=xlsx&lang=en](https://statistikdatabasen.scb.se/api/v2/savedqueries/132129/data?outputFormat=xlsx&outputFormatParams=UseCodes&lang=en )
[https://statistikdatabasen.scb.se/api/v2/savedqueries/502699/data?outputFormat=csv&lang=en](https://statistikdatabasen.scb.se/api/v2/savedqueries/132129/data?outputFormat=csv&lang=en )
[https://statistikdatabasen.scb.se/api/v2/savedqueries/502699/data?outputFormat=csv&outputFormatParams=UseCodes,SeparatorSemicolon&lang=en](https://statistikdatabasen.scb.se/api/v2/savedqueries/132129/data?outputFormat=csv&outputFormatParams=UseCodes,SeparatorSemicolon&lang=en)

Available download formats: JSON-stat2, CSV, Excel (xlsx), HTML, PX, JSON-PX, and Parquet.

However, we recommend using API queries instead of saved queries for automation. API queries are more flexible and can easily be modified later if needed.

For example, the API query could look like this:
https://statistikdatabasen.scb.se/api/v2/tables/TAB3186/data?outputFormat=xlsx&valuecodes[ContentsCode]=*&valuecodes[Tid]=2025K1,2025K2,2025K3,2025K4,2026K1,2026K2&valuecodes[Kontopost]=*&valuecodes[ExpImp]=*&heading=ContentsCode,Tid&stub=ExpImp,Kontopost&lang=en

Format: `https://statistikdatabasen.scb.se/api/v2/tables/(table number)/data(query parameters)`

How the API query is displayed depends on the selected file format and the web browser being used.
