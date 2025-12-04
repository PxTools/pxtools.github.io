# Install PxWeb 2 on your IIS server

This instruction guides you in how to install PxWeb 2 on IIS.

## Prerequisites

- A supported Windows server with IIS installed.
- The [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) must be installed on your IIS.
- ApiUrl to your PxWebApi 2 installation.
  If your tables endpoint is at `https://your.api.server/PxWeb/api/v2/tables`
  then your ApiUrl is `https://your.api.server/PxWeb/api/v2`  (omit a trailing slash, one is added automatically)

## Installation steps

1. Download the [zip file for the latest release](https://github.com/pxtools/PxWeb2/releases/latest) from GitHub.
  It is found inside the Asserts heading.
  Make sure to _Unblock_ it if its block by right clicking on it in _File Explorer_ and check the `Unblock` checkbox.
2. Put the contents of the zip file to the location of your choice. E.g `C:\inetpub\wwwroot\pxweb2`.
3. Adjust `baseApplicationPath` and `apiUrl` in `config\config.js`:

    ```js
    baseApplicationPath: "/pxweb2/"
        
    apiUrl: "your api url"
    ```

4. In `index.html`, adjust the `<base>` tag:

    ```html
    <base href="/pxweb2/">
    ```

5. In `web.config`, adjust the `Content-Security-Policy` to allow calls to your PxWebApi 2 installation. Within the `Content-Security-Policy` replace the text `https://enter-your-api-domain-here` with the domain of your PxWebApi (for example https://api.scb.se):

    ```html
    <add name="Content-Security-Policy" value="default-src 'self'; connect-src 'self' https://api.scb.se; script-src 'self'" />
    ```

1. In IIS find the folder and convert it to an application. (Consider using a separate Application pool for this)

1. Open "http(s)://"server adress"/pxweb2 .

## Troubleshooting

### PxWeb refuses to connect to PxWebApi

If you get an error message looking like this in the console of your browser:

  ```
  Refused to connect to 'https://your-api-url' because it violates the following Content Security Policy directive: "connect-src 'self' https://enter-your-api-domain-here".
  ```

#### Solution

You need to enter the domain of your PxWebApi in the Content Security Policy section in web.config, see step 5 of the installation steps above.
