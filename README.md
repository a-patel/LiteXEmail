# LiteXEmail
Abstract interface to implement any kind of basic email message services (e.g. SMTP, SendGrid, MailKit, AmazonSES, Mailgun, MailChimp, MailJet, ElasticEmail, SendinBlue)


## Add a dependency

### Nuget

Run the nuget command for installing the client as,
* `Install-Package LiteX.Email`
* `Install-Package LiteX.Email.Core`
* `Install-Package LiteX.Email.SendGrid`
* `Install-Package LiteX.Email.MailKit`
* `Install-Package LiteX.Email.AmazonSES`
* `Install-Package LiteX.Email.Mailgun`
* `Install-Package LiteX.Email.MailChimp` //COMING SOON
* `Install-Package LiteX.Email.SendinBlue` //COMING SOON
* `Install-Package LiteX.Email.MailJet` //COMING SOON
* `Install-Package LiteX.Email.ElasticEmail` //COMING SOON


## Configuration

**AppSettings**
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
    "UseDefaultCredentials": false //"--- REPLACE WITH YOUR UseDefaultCredentials (boolean) ---"
  },

  //LiteX SendGrid settings
  "SendGridConfig": {
    "SendGridApiKey": "--- REPLACE WITH YOUR SendGridApiKey ---"
  },

  //LiteX AmazonSES settings
  "AmazonSESConfig": {
    "AmazonSESAccessKey": "--- REPLACE WITH YOUR AmazonSESAccessKey ---",
    "AmazonSESSecretKey": "--- REPLACE WITH YOUR AmazonSESSecretKey ---"
  },

  //LiteX MailKit settings
  "MailKitConfig": {
    "Email": "--- REPLACE WITH YOUR Email ---",
    "DisplayName": "--- REPLACE WITH YOUR DisplayName ---",
    "Host": "--- REPLACE WITH Host Host ---",
    "Port": 587, //"--- REPLACE WITH YOUR Port (int) ---",
    "Username": "--- REPLACE WITH YOUR Username ---",
    "Password": "--- REPLACE WITH YOUR Password ---",
    "EnableSsl": false,
    "UseDefaultCredentials": false
  },

  //LiteX Mailgun settings
  "MailgunConfig": {
    "MailgunApiKey": "api:key-fakeapikey",
    "MailgunApiBaseUri": "https://api.mailgun.net/v3/",
    "MailgunRequestUri": "fakesandbox.mailgun.org/messages",
    "MailgunFrom": "postmaster@fakesandbox.mailgun.org"
  },

  //LiteX MailChimp settings
  "MailChimpConfig": {
    "MailChimpApiKey": "--- REPLACE WITH YOUR MailChimpApiKey ---"
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
    public void ConfigureServices(IServiceCollection services)
    {
        #region LiteX Email

        // Email Message (use one of below)

        #region LiteX Email (SMTP)

        // 1. Use default configuration from appsettings.json's 'SmtpConfig'
        services.AddLiteXEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXEmail(option =>
        {
            option.Host = "";
            option.Username = "";
            option.Password = "";
            option.Port = 443;
            option.Email = "";
            option.EnableSsl = true;
            option.UseDefaultCredentials = true;
            option.DisplayName = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var smtpConfig = new SmtpConfig()
        {
            Host = "",
            Username = "",
            Password = "",
            Port = 443,
            Email = "",
            EnableSsl = true,
            UseDefaultCredentials = true,
            DisplayName = ""
        };
        services.AddLiteXEmail(smtpConfig);

        #endregion

        #region LiteX Email (SendGrid)

        // 1. Use default configuration from appsettings.json's 'SendGridConfig'
        services.AddLiteXSendGridEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXSendGridEmail(option =>
        {
            option.SendGridApiKey = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var sendGridConfig = new SendGridConfig()
        {
            SendGridApiKey = ""
        };
        services.AddLiteXSendGridEmail(sendGridConfig);

        #endregion

        #region LiteX Email (MailKit)

        // 1. Use default configuration from appsettings.json's 'MailKitConfig'
        services.AddLiteXMailKitEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXMailKitEmail(option =>
        {
            option.Host = "";
            option.Username = "";
            option.Password = "";
            option.Port = 443;
            option.Email = "";
            option.EnableSsl = true;
            option.UseDefaultCredentials = true;
            option.DisplayName = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var mailKitConfig = new MailKitConfig()
        {
            Host = "",
            Username = "",
            Password = "",
            Port = 443,
            Email = "",
            EnableSsl = true,
            UseDefaultCredentials = true,
            DisplayName = ""
        };
        services.AddLiteXMailKitEmail(mailKitConfig);

        #endregion

        #region LiteX Email (AmazonSES)

        // 1. Use default configuration from appsettings.json's 'AmazonSESConfig'
        services.AddLiteXAmazonSESEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXAmazonSESEmail(option =>
        {
            option.AmazonSESAccessKey = "";
            option.AmazonSESSecretKey = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var amazonSESConfig = new AmazonSESConfig()
        {
            AmazonSESAccessKey = "",
            AmazonSESSecretKey = ""
        };
        services.AddLiteXAmazonSESEmail(amazonSESConfig);

        #endregion

        #region LiteX Email (Mailgun)

        // 1. Use default configuration from appsettings.json's 'MailgunConfig'
        services.AddLiteXMailgunEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXMailgunEmail(option =>
        {
            option.MailgunApiKey = "";
            option.ApiBaseUri = "";
            option.RequestUri = "";
            option.From = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var mailgunConfig = new MailgunConfig()
        {
            MailgunApiKey = "",
            ApiBaseUri = "",
            RequestUri = "",
            From = ""
        };
        services.AddLiteXMailgunEmail(mailgunConfig);

        #endregion

        #region LiteX Email (MailJet) - Coming Soon

        #endregion

        #region LiteX Email (ElasticEmail) - Coming Soon

        #endregion

        #region LiteX Email (MailChimp) - Coming Soon

        #endregion

        #region LiteX Email (SendinBlue) - Coming Soon

        #endregion

        #endregion
    }
}
```


## Usage

**Controller or Business layer**
```cs
/// <summary>
/// Customer controller
/// </summary>
[Route("api/[controller]")]
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
    /// Get Email Provider Type
    /// </summary>
    /// <returns></returns>
    [HttpGet]
    [Route("get-email-provider-type")]
    public IActionResult GetEmailProviderType()
    {
        return Ok(_emailSender.EmailProviderType.ToString());
    }

    /// <summary>
    /// Send email to customer
    /// </summary>
    /// <param name="customer"></param>
    /// <returns></returns>
    [HttpPost]
    [Route("send-email-to-customer")]
    public IActionResult SendEmailToCustomer(Customer customer)
    {
        try
        {
            string subject = "Welcome!",
            body = "Welcome to LiteX!",
            fromAddress = "abc@gmail.com",
            fromName = "LiteX",
            toAddress = customer.Email ?? "abc@yahoo.com",
            toName = customer.FirstName ?? "Aashish Patel",
            replyToAddress = "abc@gmail.com",
            replyToName = "Reply Name";

            IEnumerable<string> bcc = new List<string>() { "abc@outlook.com" };
            IEnumerable<string> cc = new List<string>() { "abc@gmail.com" };
            IEnumerable<Attachment> attachments = new List<Attachment>();

            _emailSender.SendEmail(subject, body, fromAddress, fromName, toAddress, toName, replyToAddress, replyToName, bcc, cc, attachments);
            //_emailSender.SendEmail(subject, body, fromAddress, fromName, toAddress, toName, replyToAddress, replyToName, bcc, cc);


            // async
            //await _emailSender.SendEmailAsync(subject, body, fromAddress, fromName, toAddress, toName, replyToAddress, replyToName, bcc, cc, attachments);


            return Ok();
        }
        catch (Exception ex)
        {

            return BadRequest(ex);
        }
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

    #endregion
}
```

### Coming soon
* Logging
