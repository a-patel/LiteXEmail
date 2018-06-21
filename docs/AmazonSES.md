# LiteX Email AmazonSES
LiteX.Email.AmazonSES is a email message library which is based on LiteX.Email.Core and AmazonSES.


## Add a dependency

### Nuget

Run the nuget command for installing the client as,
* `Install-Package LiteX.Email.Core`
* `Install-Package LiteX.Email.AmazonSES`


## Configuration

**AppSettings**
```js
{
  //LiteX AmazonSES settings
  "AmazonSESConfig": {
    "AmazonSESAccessKey": "--- REPLACE WITH YOUR AmazonSESAccessKey ---",
    "AmazonSESSecretKey": "--- REPLACE WITH YOUR AmazonSESSecretKey ---"
  }
}
```

**Startup Configuration**
```cs
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
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

        customers.Add(new Customer() { Id = 1, Username = "ashish", Email = "abc@outlook.com" });

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
