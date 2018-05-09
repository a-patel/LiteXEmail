# LiteXEmail
Abstract interface to implement any kind of basic email message services (e.g. SMTP, SendGrid, MailKit, Mailgun, MailChimp, AmazonSES, SendinBlue)


## Add a dependency

### Nuget

Run the nuget command for installing the client as,
* `Install-Package LiteX.Email`
* `Install-Package LiteX.Email.Core`



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
            //option. = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var smtpConfig = new SmtpConfig();
        services.AddLiteXEmail(smtpConfig);

        #endregion

        #region LiteX Email (SendGrid)

        // 1. Use default configuration from appsettings.json's 'SendGridConfig'
        services.AddLiteXSendGridEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXSendGridEmail(option =>
        {
            //option. = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var sendGridConfig = new SendGridConfig();
        services.AddLiteXSendGridEmail(sendGridConfig);

        #endregion

        #region LiteX Email (MailKit)

        // 1. Use default configuration from appsettings.json's 'MailKitConfig'
        services.AddLiteXMailKitEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXMailKitEmail(option =>
        {
            //option. = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var mailKitConfig = new MailKitConfig();
        services.AddLiteXMailKitEmail(mailKitConfig);

        #endregion

        #region LiteX Email (AmazonSES)

        // 1. Use default configuration from appsettings.json's 'AmazonSESConfig'
        services.AddLiteXAmazonSESEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXAmazonSESEmail(option =>
        {
            //option. = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var amazonSESConfig = new AmazonSESConfig();
        services.AddLiteXAmazonSESEmail(amazonSESConfig);

        #endregion

        #region LiteX Email (Mailgun)

        // 1. Use default configuration from appsettings.json's 'MailgunConfig'
        services.AddLiteXMailgunEmail();

        //OR
        // 2. Load configuration settings using options.
        services.AddLiteXMailgunEmail(option =>
        {
            //option. = "";
        });

        //OR
        // 3. Load configuration settings on your own.
        // (e.g. appsettings, database, hardcoded)
        var mailgunConfig = new MailgunConfig();
        services.AddLiteXMailgunEmail(mailgunConfig);

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
