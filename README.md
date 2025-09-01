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
  


## Other Observability Tools : 

- Dynatrace
- DataDog
- New Relic
- AppDynamics


# Azure Monitor

When you have critical applications and business processes that rely on azure resources, you want to monitor those resources for their availability, performance and operation. 
**Azure Monitor** is a full-stack monitoroing service that provides a complete set of features to monitor your azure resources. <br/> We can also use Azure monitor to monitor resources in other clouds and on-premises.

- Azure monitor tool we can access it in the application's menu in the azure portal.
      * Overview
      * Activity Log
      AND <br/>
      * Monitoring <br/>
<img width="2386" height="1337" alt="image" src="https://github.com/user-attachments/assets/95cb7962-3375-4e0a-b5f3-684913d6c7cf" />

## Overview Page:
The overview page includes details about the resource and often its current state. Many Azure services have **Monitoring** tab includes charts for set of key metricers. Charts are quick way to revoew the operation of the resources.

<img width="2255" height="1218" alt="image" src="https://github.com/user-attachments/assets/2c7bae3b-1303-46ab-b653-e35fcfb018af" />

## Activity Log:
The **Activity Log** menu item let you view entries in the activity log for resources.
These are subscription-level events that track operations for each Azure resource, for example, creating a new resource or starting a virtual machine. Activity log events are automatically generated and collected for viewing in Azure portal.

<img width="1217" height="628" alt="image" src="https://github.com/user-attachments/assets/3af3a09d-5f37-48ce-bc46-92d43fe25426" />

## Insights  (In Monitoring menu)

The Insights menu item opens the insight for the resource if the Azure service has one. Insights provide a customized monitoring experience built on the Azure Monitor data platform and standard features. Examples include **Application insights, Container insights, and Virtual machine insights**.

<img width="1383" height="742" alt="image" src="https://github.com/user-attachments/assets/38d95381-1a7e-47d1-98bd-d0ffe108372f" />

## Alerts:  (In Monitoring menu)

Alerts page shows you any recent alerts that were fired for the resource. Alert proactivily notify you when importent conditions are found in your monitoring data and can use data from either Mertics or logs.
We can create alert according to our needs.
<img width="2254" height="776" alt="image" src="https://github.com/user-attachments/assets/ce096524-5af0-43ed-ac5e-1176cc490243" />


## Metrics  (In Monitoring menu)

The Metrics menu item opens Metrics Explorer which allows you to analyze platform metrics for the resource. These are numerical values that are automatically collected at regular intervals and describe some aspect of a resource at a particular time. You can work with individual metrics or combine multiple metrics to identify correlations and trends. This is the same Metrics Explorer that opens when you select one of the charts on the Overview page

<img width="1218" height="741" alt="image" src="https://github.com/user-attachments/assets/4e8095ec-2548-4ac4-8f37-0ab499105370" />

## Diagnostic Settings (In Monitoring menu)

The Diagnostic settings page lets you create a diagnostic setting to collect the resource logs for your resource. Resource logs provide insight into operations that were performed by an Azure resource, such as getting a secret from a key vault or making a request to a database. Resource logs are generated automatically, but you must create a diagnostic setting to send them to a Log Analytics workspace or some other destination.

<img width="2255" height="876" alt="image" src="https://github.com/user-attachments/assets/0a58a183-d769-408e-a06c-47eeb0e9e72d" />







      
