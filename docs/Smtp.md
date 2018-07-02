# LiteX Email Smtp
> LiteX.Email is a email message library which is based on LiteX.Email.Core and Smtp.

Allow sending email messages via SendGrid.

Wrapper around Smtp api to send email messages from any type of application.

Small library for manage email with Smtp. A quick setup for Smtp.

Wrapper library is just written for the purpose to bring a new level of ease to the developers who deal with Smtp integration with your system.

## Basic Usage


### Install the package

> Install via [Nuget](https://www.nuget.org/packages/LiteX.Email/).

```Powershell
PM> Install-Package LiteX.Email
```

##### AppSettings
```js
{  
  //LiteX Smtp settings
  "SmtpConfig": {
    "Email": "--- REPLACE WITH YOUR Email ---",
    "DisplayName": "--- REPLACE WITH YOUR DisplayName ---",
    "Host": "--- REPLACE WITH Host Host ---",
    "Port": 587, //"--- REPLACE WITH YOUR Port (int) ---",
    "Username": "--- REPLACE WITH YOUR Username ---",
    "Password": "--- REPLACE WITH YOUR Password ---",
    "EnableSsl": true, //"--- REPLACE WITH YOUR EnableSsl (boolean) ---",
    "UseDefaultCredentials": false, //"--- REPLACE WITH YOUR UseDefaultCredentials (boolean) ---",
    "EnableLogging": true
  }
}
```

##### Configure Startup Class
```cs
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // 1. Use default configuration from appsettings.json's 'SendGridConfig'
        services.AddLiteXSendGridEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXSendGridEmail(option =>
        {
            option.SendGridApiKey = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var sendGridConfig = new SendGridConfig()
        {
            SendGridApiKey = "",
            EnableLogging = true
        };
        services.AddLiteXSendGridEmail(sendGridConfig);
        
        
        // add logging (optional)
        services.AddLiteXLogging();
    }
}
```

### Sample Usage Example
> Same for all providers. 

For more helpful information about LiteX Email, Please click [here.](https://github.com/a-patel/LiteXEmail/blob/master/README.md#step-3--use-in-controller-or-business-layer-memo)


