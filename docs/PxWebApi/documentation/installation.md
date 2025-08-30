# Install PxWebApi 2.0 on your IIS server
This instruction guides you in how to install PxWebApi 2.0 on a clean install of IIS in the root of the  _Default Web Site_ with PX file database.

## Prerequisites
- A supported Windows server with IIS installed you might want to enable _Dynamic Content Compression_ service for IIS to save some bandwidth.
- Have .NET 8 installed on the web server. It is recommended  to install the [Windows Hosting Bundle Installer](https://dotnet.microsoft.com/en-us/download/dotnet/8.0).

## Installation steps
- Download the [zip file for the latest release](https://github.com/pxtools/PxWebApi/releases/latest) from GitHub. Make sure to _Unblock_ it if its block by right clicking on it in _File Explorer_ and check the `Unblock` checkbox.
- Extract the zip file to the location of your choice.
- Copy the content of the `release` folder that you just extracted to `C:\inetpub\wwwroot`.
- Give the user `IIS APPPOOL\DefaultAppPoo` `Modify` and `Write` permissions to the folder  `C:\inetpub\wwwroot\wwwroot\ControllerStates`.
- Create a folder called `sq` under `C:\inetpub\wwwroot\wwwroot` and place you data in that folder. And give the user `IIS APPPOOL\DefaultAppPoo` `Modify` and `Write` permissions.
- Create a folder called `Database` under `C:\inetpub\wwwroot\wwwroot` and place you data in that folder. And give the user `IIS APPPOOL\DefaultAppPoo` `Modify` and `Write` permissions.

!!! info
    If you do not have any data of your own you can download the 
    [tiny demo database](https://github.com/PxTools/PxWebApi/raw/refs/heads/main/docker/pxwebapi/Database/tinydatabase.zip) 
    and use that as your database to test things out.

- Generate the `Menu.xml` file with the following PowerShell command.
  ```powershell
  Invoke-WebRequest -Uri 'http://localhost/api/v2/admin/database' -Method Put -Headers @{'API_ADMIN_KEY' = 'test'}
  ```
- Wait for the `Menu.xml` file to be created. It can take some time depending on the size of your database. You can run the following PowerShell command to see if the file has been created.
  ```powershell
  Invoke-WebRequest -Uri 'http://localhost/api/v2/admin/database' -Method Get -Headers @{'API_ADMIN_KEY' = 'test'} | Select-Object -Expand Content
  ```
  You will get a JSON result. Look for a _Finished at_ `Message`
  ```json
  {
    "State": "finished",
    "LastRunTime": "2025-08-29T13:54:42Z",
    "EventsFromLastRun": [
        {
            "Type": "Information",
            "Message": "Started at 8/29/2025 1:54:42 PM"
        },
        {
            "Type": "Information",
            "Message": "Menu build started 2025-08-29 15:54:42"
        },
        {
            "Type": "Information",
            "Message": "Menu build ended 2025-08-29 15:54:42"
        },
        {
            "Type": "Information",
            "Message": "Finished at 8/29/2025 1:54:42 PM"
        }
    ]
  }
  ```
- Generate the search index. The search index is used by the API to answer questions about the content in the database. Generate it by running the following PowerShell command.
  ```powershell
  Invoke-WebRequest -Uri 'http://localhost/api/v2/admin/searchindex' -Method POST -Headers @{'API_ADMIN_KEY' = 'test'}
  ```
- Wait for the search index to be created. It can take some time depending on the size of your database. You can run the following PowerShell command to see if the search index has been created.
  ```powershell
  Invoke-WebRequest -Uri 'http://localhost/api/v2/admin/searchindex' -Method Get -Headers @{'API_ADMIN_KEY' = 'test'} | Select-Object -Expand Content
  ```
  You will get a JSON result. Look for a _Finished at_ `Message`
  ```json
  {
      "State": "finished",
      "LastRunTime": "2025-08-29T14:17:50Z",
      "EventsFromLastRun": [
          {
              "Type": "Information",
              "Message": "Started at 8/29/2025 2:17:49 PM"
          },
          {
              "Type": "Information",
              "Message": "Finished at 8/29/2025 2:17:50 PM"
          }
      ]
  }
  ```
- Verify that the api is running by opening up a browser on the web server and go to [http://localhost/api/v2/tables](http://localhost/api/v2/tables). This will result in a JSON response listing the contents of your database.
