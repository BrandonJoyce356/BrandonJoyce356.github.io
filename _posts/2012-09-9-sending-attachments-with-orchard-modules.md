---
layout: post
title: Sending Attachments With Orchard Modules 
permalink: sending-attachments-with-orchard-modules 
---
The last event at Orchard Harvest 2012 was a hackathon.  I decided to sit at a table with some folks that wanted to send attachments with Orchard.  In this particular case, the email action was already being triggered by a rule. (MailActions) However, there isn't a built-in option for attaching files. So, we decided to build a simple module that would implement IMessageEventHandler to intercept the "Sending" event and append our attachment.  Here is the result of our work...

Create Module w/ Orchard Command Line

{% highlight csharp %}
c:\source\orchard\src\Orchard.Web> bin\orchard.exe
orchard> feature enable Orchard.CodeGeneration
orchard> codegen Module CustomEmailStuff
orchard> feature enable CustomEmailStuff
{% endhighlight %}

Add a Handler to Send Our Email Attachment

This is a contrived example that just uses a hard-coded file path.
{% highlight csharp %}
using System.Net.Mail;
using Orchard.Messaging.Events;
using Orchard.Messaging.Models;
 
namespace CustomEmailStuff.Events {  
  public class MailActionEventHandler : IMessageEventHandler {
    const string Picture = @"C:\Users\Username\Pictures\some-picture.jpg";

    public void Sending(MessageContext context) {
      //We know that the type will be "ActionEmail" 
      //when coming from the MailActions rule.    
      if (context.Type == "ActionEmail") {
        var attachment = new Attachment(Picture);
        context.MailMessage.Attachments.Add(attachment);
      }                
    }

    public void Sent(MessageContext context) {
    //nuthin'
    }
  }
}
{% endhighlight %}

That's it! Now any emails going out based on the email action rule will get this attachment. We didn't get much further with this, but we started to work on parameterizing the file path based on some more rules.


