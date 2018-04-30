# LiteXEmail
Abstract interface to implement any kind of basic email message services (e.g. SMTP, SendGrid, MailKit, Mailgun, MailChimp, AmazonSES, SendinBlue)


## Add a dependency

### Nuget

Run the nuget command for installing the client as,
* `Install-Package LiteX.Email`
* `Install-Package LiteX.Email.Core`
* `Install-Package LiteX.Email.SendGrid`
* `Install-Package LiteX.Email.MailKit`
* `Install-Package LiteX.Email.AmazonSES`
* `Install-Package LiteX.Email.Mailgun` //COMING SOON
* `Install-Package LiteX.Email.MailChimp` //COMING SOON
* `Install-Package LiteX.Email.SendinBlue` //COMING SOON


## Configuration

**AppSettings**
```js
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


## Usage

**Controller or Business layer**
```cs
/// <summary>
/// Customer controller
/// </summary>
public class CustomerController : Controller
{
    #region Fields
    
    private readonly IEmailSender _emailSender;

    #endregion

    #region Ctor

    /// <summary>
    /// Ctor
    /// </summary>
    /// <param name="emailSender"></param>
    public CustomerController(IEmailSender emailSender)
    {
        _emailSender = emailSender;
    }

    #endregion

    #region Methods

    /// <summary>
    /// Send email to customer
    /// </summary>
    /// <param name="customer"></param>
    /// <returns></returns>
    public IActionResult SendEmailToCustomer(Customer customer)
    {
        try
        {
            string subject = "Welcome!",
            body = "Welcome to our website!",
            fromAddress = "from@example.com",
            fromName = "Yousite",
            toAddress = customer.Email,
            toName = customer.FirstName,
            replyToAddress = "reply@example.com",
            replyToName = "Yousite";

            IEnumerable<string> bcc = new List<string>() { "bcc@example.com" };
            IEnumerable<string> cc = new List<string>() { "cc@example.com" };
            //IEnumerable<Attachment> attachments = new List<Attachment>();

            var isSent = _emailSender.SendEmail(subject, body, fromAddress, fromName, toAddress, toName, replyToAddress, replyToName, bcc, cc);
        }
        catch (Exception ex)
        {

            return BadRequest(ex);
        }
        return Ok();
    }

    #endregion

    #region Utilities

    private IList<Customer> GetCustomers()
    {
        IList<Customer> customers = new List<Customer>();

        customers.Add(new Customer() { Id = 1, Username = "ashish", Email = "toaashishpatel@outlook.com" });

        return customers;
    }

    private Customer GetCustomerById(int id)
    {
        Customer customer = null;

        customer = GetCustomers().ToList().FirstOrDefault(x => x.Id == id);

        return customer;
    }

    public static byte[] StreamToByteArray(Stream input)
    {
        byte[] buffer = new byte[16 * 1024];
        using (MemoryStream ms = new MemoryStream())
        {
            int read;
            while ((read = input.Read(buffer, 0, buffer.Length)) > 0)
            {
                ms.Write(buffer, 0, read);
            }
            return ms.ToArray();
        }
    }

    #endregion
}
```


### Coming soon

Attachment
