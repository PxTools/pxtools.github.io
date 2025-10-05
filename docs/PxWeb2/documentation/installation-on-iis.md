# Install PxWebApi 2.0 on your IIS server
This instruction guides you in how to install PxWeb 2.0 on IIS.

## Prerequisites
- A supported Windows server with IIS installed. 
- Base Url to your PxWebapi2 installation. If your tables endpoint is at "https://your.api.server/PxWeb/api/v2/tables" then your 
Base Url is at "https://your.api.server/PxWeb/api/v2" 

## Installation steps
- Download the [zip file for the latest release](https://github.com/pxtools/PxWeb2/releases/latest) from GitHub.
  It is found inside the Asserts heading.
  Make sure to _Unblock_ it if its block by right clicking on it in _File Explorer_ and check the `Unblock` checkbox.
- Put the contents of the zip file to the location of your choice. E.g `C:\inetpub\wwwroot\pxweb2`.
- The 

1. Set `baseApplicationPath` in `config\config.js`:

   ```js
   baseApplicationPath: "/pxweb2/"
   ```
   Just this line below is the apiUrl, make it point to your base url (omit a trailing slash, one is added automatically).

2. In `index.html`, adjust the `<base>` tag:

   ```html
   <base href="/pxweb2/">
   ```

(I didnt need this: 3. Ensure the web server rewrites (if needed) static file requests correctly to that subpath.) 

- In IIS find the folder and convert it to application. ( Concider using a separate Application pool for this :-) )

- Open "http(s)://"server adress"/pxweb2 . 
