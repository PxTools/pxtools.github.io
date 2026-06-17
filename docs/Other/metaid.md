# Links from a dataset
A table may have links to external resources, such as definitions in a classification database or a statistics home page. PxWebApi needs to provide links to these resources.

The use case is to provide content in the Information pane of the PxWeb gui:

  ![Information pane in PxWeb](MetaIdInGUI_1.png?raw=true)

If you click on "Statistics page" you go to https://www.ssb.no/en/arblonn . 
The response from the API contains the following information for this link:

```
 "link": {
    "related": [
      {
        "extension": {
          "relation": "statistics-homepage",
          "metaid": "KORTNAVN:arblonn"
        },
        "href": "https://www.ssb.no/en/arblonn",
        "label": "Statistics page",
        "type": "text/html"
      },
```

So information for a link includes:
```
href:  a URL e.g. "https://www.ssb.no/en/arblonn" , pointing to a resource
label: text describing the resource, like "Statisticspage". 
relation: some description of how the resource is related to the table , like "statistics-homepage". 

type: the MIME type of the resource, typically "text/html", but could also be e.g. markdown. 
metaid: the META-ID that is the source of the link, e.g. "KORTNAVN:arblonn". 
```


The datasource of the API has a property called META-ID. You can attach a META-ID to a table, variable or value to make the API generate a link on the table, a variable or a value.
The META-ID values are urns e.g. "KORTNAVN:arblonn". To attach more than one urn, separate them with a comma. 

The API transforms it to href,type,label and relation according to rules
given in a config file called metaid.config.

## metaid.config
For links to metadata systems shall be created, the metaid.config file must be placed in the root directory of the application. 
In metaid.config the rules are divided into three sections in the metaid.config file for attachments at tables, variables and values. The sections are be named:
* onTable
* onVariable
* onValue

A "rule" is called a metaSystem and is identified by an ID which is a string.
When the API finds a META-ID, it iterates through  all metasystems for the attachment level and checks whether the META-ID starts with the ID of the metasystem.  If it matches, one or more links are generated.

The rest of the META-ID after the ID of the metasystem is considered to be arguments. If the META-ID value is KORTNAVN:arblonn and there is a metasystem with ID KORTNAVN, the argument is arblonn. 
If the META-ID value is KORTNAVN:arblonn:arg2 and there is a metasystem with ID KORTNAVN, the arguments are arblonn and arg2.

In addition to the ID, each Metadata system has configuration for 
* relation to the table, variable or value it is attached to 
* Language
* Label (text) format
* URL format

The URL format normally contains placeholders {0}, {1} … {n} where the arguments from the META-ID will be inserted. 
The label format may contain placeholders depending on the attachment level, for values {0} will be replaced by the name of the variable and {1} by the name of the value.
For variables it's just {0} for the name of the variable. For label formats in the onTable section there are no placeholders.
(A metasystem may contain the name of a labelsFile. This is for cases where a good text cannot be generated and needs to be handwritten.)

For example this fragment of the config file:
```
<?xml version="1.0"?>
<metaId>
  <onTable>
    <metaSystem id="KORTNAVN">
      <relationalGroup relation="statistics-homepage" type="text/html">
        <link pxLang="no" labelStringFormat="Statistikkens hjemmeside" urlStringFormat="https://www.ssb.no/{0}" />
        <link pxLang="en" labelStringFormat="Statistics page" urlStringFormat="https://www.ssb.no/en/{0}" />
      </relationalGroup>
     ... 
 
```

When the API finds this META-ID "KORTNAVN:arblonn", on table level, it creates
```
 "link": {
    "related": [
      {
        "extension": {
          "relation": "statistics-homepage",
          "metaid": "KORTNAVN:arblonn"
        },
        "href": "https://www.ssb.no/en/arblonn",
        "label": "Statistics page",
        "type": "text/html"
      },
```
for an English request.


Here is a full metaid.config:
```
<?xml version="1.0"?>
<metaId labelFilesFolder="wwwroot\cnmm-database\metaid">
  <onTable>
    <metaSystem id="KORTNAVN">
      <relationalGroup relation="statistics-homepage" type="text/html">
        <link pxLang="no" labelStringFormat="Statistikkside" urlStringFormat="https://www.ssb.no/{0}" />
        <link pxLang="en" labelStringFormat="Statistics page" urlStringFormat="https://www.ssb.no/en/{0}" />
      </relationalGroup>
      <relationalGroup relation="about-statistics" type="text/html">
         <link pxLang="no" labelStringFormat="Definisjoner og forklaringer" urlStringFormat="https://www.ssb.no/{0}#om-statistikken" />
         <link pxLang="en" labelStringFormat="Definitions and explanations" urlStringFormat="https://www.ssb.no/en/{0}#om-statistikken" />
      </relationalGroup>
    </metaSystem>
  </onTable>
  <onVariable>
    <metaSystem id="urn:ssb:classification:klass">
       <relationalGroup relation="definitions" type="text/html">   
         <link pxLang="no" labelsFile="klass_map_no.txt" labelStringFormat="Klassifikasjon for {0}." urlStringFormat="https://www.ssb.no/klass/klassifikasjoner/{0}" />
         <link pxLang="en" labelsFile="klass_map_en.txt" labelStringFormat="Classification for {0}." urlStringFormat="https://www.ssb.no/en/klass/klassifikasjoner/{0}" />
       </relationalGroup>  
    </metaSystem>
    <metaSystem id="urn:ssb:conceptvariable:vardok">
      <relationalGroup relation="definitions" type="text/html">   
         <link pxLang="no" labelStringFormat="Variabeldefinisjon av {0}" urlStringFormat="https://www.ssb.no/a/metadata/conceptvariable/vardok/{0}/nb" />
         <link pxLang="en" labelStringFormat="Variable definition of {0}" urlStringFormat="https://www.ssb.no/a/metadata/conceptvariable/vardok/{0}/en" />
      </relationalGroup>
    </metaSystem>
  </onVariable>
  <onValue>
    <metaSystem id="urn:ssb:conceptvariable:vardok">
      <relationalGroup relation="definitions" type="text/html">
        <link pxLang="no" labelStringFormat="Variabeldefinisjon av {1}" urlStringFormat="https://www.ssb.no/a/metadata/conceptvariable/vardok/{0}/nb" />
        <link pxLang="en" labelStringFormat="Variable definition of {1}" urlStringFormat="https://www.ssb.no/a/metadata/conceptvariable/vardok/{0}/en" />
      </relationalGroup>
    </metaSystem>
    <metaSystem id="urn:ssb:contextvariable:common">
      <relationalGroup relation="definitions" type="text/html">
        <link pxLang="no" labelStringFormat="Variabeldefinisjon av {1} (KOSTRA)" urlStringFormat="https://www.ssb.no/kompis/statbank/?id={0}&amp;ver={1}&amp;val={2}&amp;lang=no" />
        <link pxLang="en" labelStringFormat="Variable definition of {1} (KOSTRA)" urlStringFormat="https://www.ssb.no/kompis/statbank/?id={0}&amp;ver={1}&amp;val={2}&amp;lang=en" />
      </relationalGroup>  
    </metaSystem>
  </onValue>
</metaId>

```

Note: 
* The GUI looks for links with relation "statistics-homepage", "about-statistics" and "definitions" to show in the Information pane.
* The metaSystem KORTNAVN creates 2 links per language. 
* The last metaSystem on value does not use the name of the variable, only the name of the value.

### Labels file
```
<metaId labelFilesFolder="wwwroot\cnmm-database\metaid">
```
and
```
<onVariable>
    <metaSystem id="urn:ssb:classification:klass">
       <relationalGroup relation="definitions" type="text/html">   
         <link pxLang="no" labelsFile="klass_map_no.txt"
```
work together to specify a file wwwroot\cnmm-database\metaid\klass_map_no.txt

These files look like: 
```
urn:ssb:classification:klass:1 Standard for delområde- og grunnkretsinndeling
urn:ssb:classification:klass:2 Standard for kjønn
urn:ssb:classification:klass:3 Standard for gruppering av hjelpe- og omsorgstiltak i barnevernet
urn:ssb:classification:klass:4 Standard for regionale spesialkoder
urn:ssb:classification:klass:5 Standard for PRODCOM koder
```
If the META-ID is found here the text after the urn is used and  the labelStringFormat is ignored.

