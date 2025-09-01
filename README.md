# logging
Different type of loging

## Logging into File System
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


## Logging to Blob Storage

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

### Notes:
We log our logs in File system or Blob now. When we have any problem, we need to search the log file or blob to identify the problem. Searching into a large amount of file or blob is very dificult.
So we can use **Log stream** of Azure application.
   * How to enable Log stream
       - Go to Azure application **Monitoring**
       - Make sure **File log** or **Blob Log** is enabled
       - Select Log Stream, This will open a command prompt and connected to any enabled Logging system  (File log or Blob log)
       - In the command window we can see the live logging while our application runs and if there is any logs
    
<img width="1349" height="573" alt="image" src="https://github.com/user-attachments/assets/ff4cf908-8c7d-49ae-909b-86c90356bd3c" />

         

       - 

## Login to Application Insights
- For simple and small applications, Logging into File system or Blob storage is fine, but If we are working on Complex and large application, it is better logging into Application Insight
- It is not only Logging tool but also it is an extension of Azure Monitor
  ### How to enable Application insight
  1. Create Application insight fr out application in Azure
  2. Go to application's Application insight -> Configure -> Properties  -> Copy Connection String
  3. Add the connection string into our application appsettings.json
     <pre>
       "ConnectionStrings" : {
         "AppInsights" : "InstrumentationKey..."
       }
     </pre>
  4.  In the application progeam.cs, we need to configure ApplicationInsights
     <pre>
       builder.Logging.AddApplicationInsights(
          configureTelemetryConfiguration: (config) => {
             confog.ConnectionString = builder.Configuration.GetConnectionString("AppInsights"), configureApplicationInsightsLoggerOptions: (options) => { }
          }
       )
     </pre>
  5. Now everything is ready we can run the application and check the ApplicationInsight logg
  6. Goto Application's ApplicationInsights -> Monitoring -> Logs
  7. We need to use see the Logs we need to use "**Kusto query language (KQL)**", We can select duration in the dropdown options
  8. Sample KQL :
       - traces  limit 100
  
