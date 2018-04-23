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

        // add smtp config settings
        services.AddSingleton(configuration.GetSection("SmtpConfig").Get<SmtpConfig>());

        // register smtp email sender service
        services.AddTransient<IEmailSender, EmailSender>();

        #endregion

        #region LiteX Email (SendGrid)

        // add send grid config settings
        services.AddSingleton(configuration.GetSection("SendGridConfig").Get<SendGridConfig>());

        // register send grid email sender service
        services.AddTransient<IEmailSender, SendGridEmailSender>();

        #endregion

        #region LiteX Email (MailKit)

        // add MailKit config settings
        services.AddSingleton(configuration.GetSection("MailKitConfig").Get<MailKitConfig>());

        // register MailKit email sender service
        services.AddTransient<IEmailSender, MailKitEmailSender>();

        #endregion

        #region LiteX Email (AmazonSES)

        // add AmazonSES config settings
        services.AddSingleton(configuration.GetSection("AmazonSESConfig").Get<AmazonSESConfig>());

        // register AmazonSES email sender service
        services.AddTransient<IEmailSender, AmazonSESEmailSender>();

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
