# LiteXEmail
> LiteXEmail is simple yet powerful and very high-performance sms mechanism and incorporating both synchronous and asynchronous usage with some advanced usages which can help us to handle sending email more easier!
> Provide Email service for any type of application (.NET Core, .NET Standard).

Very simple yet advanced configuration. Minimal (one line) code configuration is required.

Abstract interface to implement any kind of basic email message services (e.g. SMTP, SendGrid, MailKit, Mailgun, MailChimp, AmazonSES, SendinBlue)
      
It supports various sms providers and implements many advanced features. You can also write your own and extend it also extend existing providers.

Easily migrate or switch between one to another provider with no code breaking changes.

Having a default/generic implementation to wrap the SMTP, SendGrid, MailKit, Mailgun, MailChimp, AmazonSES, SendinBlue and independed on the underlying provider SDK(s).

The Core library contains all base interfaces and tools. One should install at least one other LiteXEmail package to get email handle implementations.

This is the ASP.NET Core configuration integration package (Built-in).



## Email Providers :books:
- [Smtp](docs/Smtp.md)
- [SendGrid](docs/SendGrid.md)
- [AmazonSES](docs/AmazonSES.md)
- [MailKit](docs/MailKit.md)
- [Mailgun](docs/Mailgun.md)
- [MailJet](docs/MailJet.md) - coming soon
- [MailChimp](docs/MailChimp.md) - coming soon
- [SendinBlue](docs/SendinBlue.md) - coming soon
- [ElasticEmail](docs/ElasticEmail.md) - coming soon



## Features :pager:
- Attachment
- ReplyTo
- Cc, Bcc (multiple)
- Async compatible
- Thread safe, concurrency ready
- Interface based API to support the test driven development and dependency injection
- Leverages a provider model on top of ILiteXEmailSender under the hood and can be extended with your own implementation



## Basic Usage :page_facing_up:


### Step 1 : Install the package :package:

> Choose one kinds of email provider type that you needs and install it via [Nuget](https://www.nuget.org/profiles/iamaashishpatel).
> To install LiteXEmail, run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)

```Powershell
PM> Install-Package LiteX.Email
PM> Install-Package LiteX.Email.SendGrid
PM> Install-Package LiteX.Email.AmazonSES
PM> Install-Package LiteX.Email.MailKit
PM> Install-Package LiteX.Email.Mailgun
```


### Step 2 : Configuration 🔨 
> Different types of email provider have their own way to config.
> Here are samples that show you how to config.

##### 2.1 : AppSettings 
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
  },

  //LiteX SendGrid settings
  "SendGridConfig": {
    "SendGridApiKey": "--- REPLACE WITH YOUR SendGridApiKey ---",
    "EnableLogging": true
  },

  //LiteX AmazonSES settings
  "AmazonSESConfig": {
    "AmazonSESAccessKey": "--- REPLACE WITH YOUR AmazonSESAccessKey ---",
    "AmazonSESSecretKey": "--- REPLACE WITH YOUR AmazonSESSecretKey ---",
    "AmazonRegion": "--- REPLACE WITH YOUR AmazonRegion ---",
    "EnableLogging": true
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
    "UseDefaultCredentials": false,
    "EnableLogging": true
  },

  //LiteX Mailgun settings
  "MailgunConfig": {
    "ApiKey": "api:key-fakeapikey",
    "ApiBaseUri": "https://api.mailgun.net/v3/",
    "RequestUri": "fakesandbox.mailgun.org/messages",
    "From": "postmaster@fakesandbox.mailgun.org",
    "EnableLogging": true
  },

  //LiteX MailChimp settings
  "MailChimpConfig": {
    "MailChimpApiKey": "--- REPLACE WITH YOUR MailChimpApiKey ---",
    "EnableLogging": true
  },

  //LiteX SendinBlue settings
  "SendinBlueConfig": {
    "SendinBlueApiKey": "--- REPLACE WITH YOUR  ---",
    "EnableLogging": true
  }
}
```

##### 2.2 : Configure Startup Class
```cs
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
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
            option.EnableLogging = true;
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
            DisplayName = "",
            EnableLogging = true
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
            option.EnableLogging = true;
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
            DisplayName = "",
            EnableLogging = true
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
            option.AmazonRegion = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var amazonSESConfig = new AmazonSESConfig()
        {
            AmazonSESAccessKey = "",
            AmazonSESSecretKey = "",
            AmazonRegion = "",
            EnableLogging = true
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
            option.ApiKey = "";
            option.ApiBaseUri = "";
            option.RequestUri = "";
            option.From = "";
            option.EnableLogging = true;
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var mailgunConfig = new MailgunConfig()
        {
            ApiKey = "",
            ApiBaseUri = "",
            RequestUri = "",
            From = "",
            EnableLogging = true
        };
        services.AddLiteXMailgunEmail(mailgunConfig);

        #endregion


        // add logging (optional)
        services.AddLiteXLogging();
    }
}
```

### Step 3 : Use in Controller or Business layer :memo:

```cs
/// <summary>
/// Customer controller
/// </summary>
[Route("api/[controller]")]
public class CustomerController : Controller
{
    #region Fields

    private readonly ILiteXEmailSender _emailSender;

    #endregion

    #region Ctor

    /// <summary>
    /// Ctor
    /// </summary>
    /// <param name="emailSender"></param>
    public CustomerController(ILiteXEmailSender emailSender)
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
            fromAddress = "acc.aashishpatel@gmail.com",
            fromName = "LiteX",
            toAddress = customer?.Email ?? "patelashish_90@yahoo.com",
            toName = customer?.FirstName ?? "Aashish Patel",
            replyToAddress = "aashish.mrcool@gmail.com",
            replyToName = "LiteX Reply";

            IEnumerable<string> bcc = new List<string>() { "toaashishpatel@outlook.com" };
            IEnumerable<string> cc = new List<string>() { "toaashishpatel@gmail.com" };
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

    /// <summary>
    /// Send email to customer with attachment
    /// </summary>
    /// <param name="customer"></param>
    /// <param name="file">attachment</param>
    /// <returns></returns>
    [HttpPost]
    [Route("send-email-to-customer-with-attachment")]
    [AddSwaggerFileUploadButton]
    public IActionResult SendEmailToCustomerWithAttachment(Customer customer, IFormFile file)
    {
        try
        {
            //Customer customer = null;

            string subject = "Welcome!",
            body = "Welcome to LiteX!",
            fromAddress = "acc.aashishpatel@gmail.com",
            fromName = "LiteX",
            toAddress = customer?.Email ?? "patelashish_90@yahoo.com",
            toName = customer?.FirstName ?? "Aashish Patel",
            replyToAddress = "aashish.mrcool@gmail.com",
            replyToName = "LiteX Reply";

            var attachment = new Attachment()
            {
                ContentType = file?.ContentType,
                Filename = file?.FileName,
                Data = file?.OpenReadStream()
            };

            IEnumerable<string> bcc = new List<string>() { "toaashishpatel@outlook.com" };
            IEnumerable<string> cc = new List<string>() { "toaashishpatel@gmail.com" };
            List<Attachment> attachments = new List<Attachment>();
            attachments.Add(attachment);


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


## Todo List :clipboard:

#### Storage Providers

- [x] Smtp
- [x] SendGrid
- [x] AmazonSES
- [x] MailKit
- [x] Mailgun
- MailJet
- MailChimp
- SendinBlue
- ElasticEmail


#### Basic Storage API

- [x] SendEmail



#### Coming soon
* Bulk Email


---



## Support :telephone:
> Reach out to me at one of the following places!

- Email :envelope: at <a href="mailto:toaashishpatel@gmail.com" target="_blank">`toaashishpatel@gmail.com`</a>
- NuGet :package: at <a href="https://www.nuget.org/profiles/iamaashishpatel" target="_blank">`@iamaashishpatel`</a>



## Authors :boy:

* **Ashish Patel** - [A-Patel](https://github.com/a-patel)


##### Connect with me

| Linkedin | GitHub | Facebook | Twitter | Instagram | Tumblr | Website |
|----------|----------|----------|----------|----------|----------|----------|
| [![linkedin](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-linkedin.svg)](https://www.linkedin.com/in/iamaashishpatel) | [![github](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-github.svg)](https://github.com/a-patel) | [![facebook](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-facebook.svg)](https://www.facebook.com/aashish.mrcool) | [![twitter](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-twitter.svg)](https://twitter.com/aashish_mrcool) | [![instagram](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-instagram.svg)](https://www.instagram.com/iamaashishpatel/) | [![tumblr](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-tumblr.svg)](https://iamaashishpatel.tumblr.com/) | [![website](https://cdnjs.cloudflare.com/ajax/libs/foundicons/3.0.0/svgs/fi-social-blogger.svg)](http://aashishpatel.co.nf/) |
| | | | | | |



## Donations :dollar:

[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/iamaashishpatel)



## License :lock:

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

