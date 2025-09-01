# logging
Different type of loging

- Logging to File system  -> Enble Application Logging (Filesystem) in Azure applications - Monitoring - App Service Logs

  ### Enable Log into file system
     1. In the azure Application, goto **App Service logs**
     2. Enable Application Logging(File System)
     3. Select the Log Level   (may be Information)
 
        
      * Microsoft.Extensions.Logging.ApplicationInsights
      * Microsoft.Extensions.Logging.AzureAppServices
      **Code:** In programs.cs ->


        <pre>
          builder.Logging.AddAzureWebAppDiagnostics();
          builder.Services.Cofigure<AzureFileLoggerOptions>(options =>
          {
            options.FileName = "logs-";
            options.FileSizeLimit = 50 * 1024;
            options.RetainedFileConuntLimit = 5;
          }
        </pre>

        **In appsettings.json**  <br/>


        <pre>
          "Logging" : {
              "LogLevel" : {
                  "Default": "Information"
                  "Microsoft" : "Information"
              },
              "AzureAppServicesFile:{
                  "LogLevel"{
                      "Microsoft": "None"   --> Microsoft, VS, ASP.NET Core system Logs will be avoided"
                    }
              }
          },
          "AllowedHosts" : "*"
      </pre>
  


    ~ Now if we deploy the application to azure, it will log loggs to the file system <br/>
    ~ Brwse to Application's advance tools under Deployment Tools in the left side menu  -> Click Go in the right hand screen  -> Will open in separate window -> 
 
  <img width="1140" height="650" alt="image" src="https://github.com/user-attachments/assets/d62a3e26-3f2d-4264-b7f2-b8e4cbde85c8" />

    ~ Go to **Debug Console** and select  **CMD or Powershell**   - 
   <img width="1271" height="692" alt="image" src="https://github.com/user-attachments/assets/665c3f30-8bd2-48be-ae89-f34dae73a416" />

      ~ Click on **LogFiles and then Application**    here we can see all our loggs
      ~ Logiging to File system is not appripreate when we host our application in Low pricing tire.
   

<img width="996" height="599" alt="image" src="https://github.com/user-attachments/assets/a8d45fb8-1a7b-456c-a3cc-e698cb39bbaa" />



  
- Logging to Blob Storage
  1. Enable ** Application logging (Blob)**  in **Azure application -> App Service logs**
  2. Select log Level
  3. Configure **Storage Containers** 

  4. program.cs
  <pre>
    builder.Logging.AddAzureWebAppDiagnostics();
    builder.Services.Configure<AzureBlobLoggerOptions> (options =>
      {
        options.BlobName = "log.txt";
      }
  </pre>

  5. Goto appsettings.json
     <pre>
       "AzureAppServiceBlob": {
         "LogLevel":{
             "Microsoft" : "None"
         }
       }
     </pre>

  6. After deploying the application, to see the log goto Azure application resource group ->  Select the Storage account -> Container  -> open the folder -> [datewise log] 

- Login to Application Insights
- 
