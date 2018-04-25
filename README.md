# LiteXEmail
Abstract interface to implement any kind of basic email message services (e.g. SMTP, SendGrid, MailKit, Mailgun, MailChimp, AmazonSES, SendinBlue)


## Add a dependency

### nuget

Run the nuget command for installing the client as,
`Install-Package LiteX.Email`
`Install-Package LiteX.Email.Core`
`Install-Package LiteX.Email.SendGrid`
`Install-Package LiteX.Email.MailKit`
`Install-Package LiteX.Email.AmazonSES`
`Install-Package LiteX.Email.MailChimp`
`Install-Package LiteX.Email.Mailgun`
`Install-Package LiteX.Email.SendinBlue`


## Usage

### Configuration

**AppSettings**
```json
{
  //LiteX Smtp settings
  "SmtpConfig": {
    "Email": "--- REPLACE WITH YOUR Email ---",
    "DisplayName": "--- REPLACE WITH YOUR DisplayName ---",
    "Host": "--- REPLACE WITH Host Host ---",
    "Port": "--- REPLACE WITH YOUR Port (int) ---",
    "Username": "--- REPLACE WITH YOUR Username ---",
    "Password": "--- REPLACE WITH YOUR Password ---",
    "EnableSsl": "--- REPLACE WITH YOUR EnableSsl (boolean) ---",
    "UseDefaultCredentials": "--- REPLACE WITH YOUR UseDefaultCredentials (boolean) ---"
  },

  //LiteX SendGrid settings
  "SendGridConfig": {
    "SendGridApiKey": "--- REPLACE WITH YOUR SendGridApiKey ---"
  },

  //LiteX MailKit settings
  "MailKitConfig": {
    "Email": "--- REPLACE WITH YOUR Email ---",
    "DisplayName": "--- REPLACE WITH YOUR DisplayName ---",
    "Host": "--- REPLACE WITH Host Host ---",
    "Port": "--- REPLACE WITH YOUR Port (int) ---",
    "Username": "--- REPLACE WITH YOUR Username ---",
    "Password": "--- REPLACE WITH YOUR Password ---",
    "EnableSsl": "--- REPLACE WITH YOUR EnableSsl (boolean) ---",
    "UseDefaultCredentials": "--- REPLACE WITH YOUR UseDefaultCredentials (boolean) ---"
  },

  //LiteX AmazonSES settings
  "AmazonSESConfig": {
    "AmazonSESAccessKey": "--- REPLACE WITH YOUR AmazonSESAccessKey ---",
    "AmazonSESSecretKey": "--- REPLACE WITH YOUR AmazonSESSecretKey ---"
  },

  //LiteX MailChimp settings
  "MailChimpConfig": {
    "MailChimpApiKey": "--- REPLACE WITH YOUR MailChimpApiKey ---"
  },

  //LiteX Mailgun settings
  "MailgunConfig": {
    "MailgunApiKey": "--- REPLACE WITH YOUR MailgunApiKey ---"
  },

  //LiteX SendinBlue settings
  "SendinBlueConfig": {
    "SendinBlueApiKey": "--- REPLACE WITH YOUR  ---"
  }
}
```

**Startup Configuration**
```cs
public class Startup
{
    public IConfiguration configuration { get; }

    public Startup(IConfiguration configuration)
    {
        this.configuration = configuration;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        #region LiteX Email

        // Email Message (use one of below)

        #region LiteX Email (SMTP)

        services.AddLiteXEmail(configuration);

        // OR
        // load configuration settings on your own.
        // from appsettings, database, hardcoded etc.
        var smtpConfig = new SmtpConfig();
        services.AddLiteXEmail(configuration, smtpConfig);

        #endregion

        #region LiteX Email (SendGrid)

        services.AddLiteXSendGridEmail(configuration);

        // OR
        // load configuration settings on your own.
        // from appsettings, database, hardcoded etc.
        var sendGridConfig = new SendGridConfig();
        services.AddLiteXSendGridEmail(configuration, sendGridConfig);

        #endregion

        #region LiteX Email (MailKit)

        services.AddLiteXMailKitEmail(configuration);

        // OR
        // load configuration settings on your own.
        // from appsettings, database, hardcoded etc.
        var mailKitConfig = new MailKitConfig();
        services.AddLiteXMailKitEmail(configuration, mailKitConfig);

        #endregion

        #region LiteX Email (AmazonSES)

        services.AddLiteXAmazonSESEmail(configuration);

        // OR
        // load configuration settings on your own.
        // from appsettings, database, hardcoded etc.
        var amazonSESConfig = new AmazonSESConfig();
        services.AddLiteXAmazonSESEmail(configuration, amazonSESConfig);

        #endregion

        #region LiteX Email (MailChimp) - NOT IMPLEMENTED

        //// add MailChimp config settings
        //services.AddSingleton(configuration.GetSection("MailChimpConfig").Get<MailChimpConfig>());

        //// register MailChimp email sender service
        //services.AddTransient<IEmailSender, MailChimpEmailSender>();

        #endregion

        #region LiteX Email (Mailgun) - NOT IMPLEMENTED

        //// add Mailgun config settings
        //services.AddSingleton(configuration.GetSection("MailgunConfig").Get<MailgunConfig>());

        //// register Mailgun email sender service
        //services.AddTransient<IEmailSender, MailgunEmailSender>();

        #endregion

        #region LiteX Email (SendinBlue) - NOT IMPLEMENTED

        //// add SendinBlue config settings
        //services.AddSingleton(configuration.GetSection("SendinBlueConfig").Get<SendinBlueConfig>());

        //// register SendinBlue email sender service
        //services.AddTransient<IEmailSender, SendinBlueEmailSender>();

        #endregion

        #endregion
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {

    }
}
```

### Coming soon

Attachment
