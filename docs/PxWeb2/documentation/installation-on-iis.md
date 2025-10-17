# Install PxWebApi 2 on your IIS server

This instruction guides you in how to install PxWeb 2 on IIS.

## Prerequisites

- A supported Windows server with IIS installed.
- ApiUrl to your PxWebapi2 installation.
  If your tables endpoint is at `https://your.api.server/PxWeb/api/v2/tables`
  then your ApiUrl is `https://your.api.server/PxWeb/api/v2`  (omit a trailing slash, one is added automatically)

## Installation steps

1. Download the [zip file for the latest release](https://github.com/pxtools/PxWeb2/releases/latest) from GitHub.
  It is found inside the Asserts heading.
  Make sure to _Unblock_ it if its block by right clicking on it in _File Explorer_ and check the `Unblock` checkbox.
1. Put the contents of the zip file to the location of your choice. E.g `C:\inetpub\wwwroot\pxweb2`.
1. Adjust `baseApplicationPath` and `apiUrl` in `config\config.js`:

    ```js
    baseApplicationPath: "/pxweb2/"
        
    apiUrl: "your api url"
    ```

1. In `index.html`, adjust the `<base>` tag:

    ```html
    <base href="/pxweb2/">
    ```

1. I didnt need this: Ensure the web server rewrites (if needed) static file requests correctly to that subpath.

1. In IIS find the folder and convert it to application. ( Concider using a separate Application pool for this :-) )

1. Open "http(s)://"server adress"/pxweb2 .
