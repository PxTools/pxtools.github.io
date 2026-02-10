#Saved queries in PxWeb2 and PxWebApi2.
A saved query has an output format, which was selected when it was created.

If the output format is "Screen" you should get the table with your selections in the gui. 
If the output is "Download" (e.g. excel) the browser should download the data in the chosen format.

In the old classic version, the url of the gui was used for both "screen" and "download", and the output format "screen"  got 
you pxweb "as it was" when the query was created. If the output format was excel, you would not see the gui, the browser would just start to download a file.

In the new version, the responsibillity is sharded between the api and and gui:
The url to the gui is used to get pxweb "as it was", also for "download" output formats.
The url to the api is used to download  the data of any savedquery. For the "sceen" output format will download jsonstat2 files. 

`http://mysite/api/pxwebapi/v2/savedqueries/N/data`  will download the file.
`http://mysite/api/pxwebapi/v2/savedqueries/N` will get you the definition of the savedquery.
