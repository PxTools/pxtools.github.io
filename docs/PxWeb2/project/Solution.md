# Is PxWeb right for you?
PxWeb is open source and can be downloaded and customized without charge. However, this installation requires IT-skills. Also, to publish statistics in PxWeb you need to create px-files, either by technically integrating with your own IT environment using Python, R or similar, or by manually producing the files using the tool [PxEdit](https://stat.fi/tup/tilastotietokannat/px-tuoteperhe_en.html) Another solution is to use an SQL database, but this also requires in-house technical support. 

## PxWeb might be right for your organization if: 
### 1. Your tables are multidimensional
Unlike most open source data tools, PxWeb is custom made to deal with:
- multidimensional tables
- official metadata
- classifications and hierarchies
- table building
PxWeb is specially designed for disseminating official statistics and handles this without the need for adaptations.

### 2. You want an analytical tool and not just static tables
Your users can among other things:
- Filter variables
- Pivot the table
- Make their own extractions
- Compare categories
Many open source tools only give the user static tables

You want an automatic, metadata rich, built-in API for all your tables and datasets
Every time you publish a table in PxWeb, you also get an API. This is one of the main reasons economists, researchers, data scientists and financial journalists like PxWeb. 

### 3. You want mobile friendly data dissemination
Many argue that accessing official statistics and other multidimensional tables requires expert knowledge and a computer. We beg to differ. PxWeb provides users with an easy way to look up tables and data on the go using their mobile devices.

### 4. You want a format which is tailor made for statistics
[The Px-format](https://www.scb.se/en/services/statistical-programs-for-px-files/px-file-format/) is: 
- Structured for statistics
- Easy to version
- Rich on metadata
- Stable over time
- High perfomance. Px-files usually offer better performance than other, more complex, data formats such as SDMX. 
Other solutions often require conversion to CSV/JSON without metadata, or complex data models. The px-format is what many described as developer friendly. It should be noted that for extremely large datasets, the px-format might be limited and you will need to establish another solution for that dataset. 

### 5. You want a stable, easy and low maintenance application
Pxweb is: 
- Easy to install
- Easy to run
- Lightweight and requires limited server capacity
- Good performance
However, if you have a high number of datasets, manually producing it in PxEdit can be time consuming. To integrate with internal systems and to automate px-file production will require an IT investment. 

### 6. You want open source code, no licence fees and a chance to influence further development
- PxWeb is open source and several public organisations such as Statistics Sweden and Statistics Norway are collaborating in its continual development. This ensures longevity and predictability.
- Through our web community you can track what new features and changes are coming in the next release. You can also discuss technical issues you might have and propose new features. You can also attend our annual international Px-meeting.
- The metadata rich PxWebApi makes it easy for you to develop customized applications if you have specific needs. 

### 7. If you need a multilanguage dissemination system 
Many public institutions and statistical agencies are required to support several languages. PxWeb supports this out of the box, whereas a lot of other applications requires more manual work, adaptations in existing publishing solutions etc. 

