---
layout: post
title: Sending Emails With Orchard Modules
category: Orchard CMS
permalink: sending-emails-with-orchard-modules
---
Sending an email in .Net is pretty simple. You just put some settings into an instance of SmtpClient, create a MailMessage and off you go. In Orchard CMS, there are a couple of other ways. While you could still use the SmtpClient directly, there are benefits to doing it the Orchard way.  The first option is to use the new Forms, Tokens, and Rules modules as outlined here: http://www.davidhayden.me/blog/rules-tokens-and-form-api-in-orchard-1.3  This is kind of a content driven approach.  The second option which I will be outlining, is more along the lines of what a Module developer would use.  In fact, option #1 is built on this method!

There are a few abstractions involved in this process.  Let's have a look.

*IMessagingChannel*

An implementation of IMessagingChannel is responsible for the low level details of sending a message. If you take a peak into the Orchard.Email module, you'll see an implementation for an email channel. You'll see that it's using the SmtpClient and handling config values for you. It also does some other things like logging etc. The advantage of all of this is that you could possibly change your message channel by implementing your own. Maybe you have a CRM you need to integrate with or maybe you use some other type of messaging service. Basically, we have freedom to change this behavior without mucking with all of our modules that send emails. Since we just want to send an email, we don't need to implement a channel, it's already done.

*IMessageManager*

The IMessageManager interface is what you use to actually trigger sending a message from your module. For example, if you wanted to send an email from your module, it might look something like this.
{% highlight csharp %}
namespace MyModule.Services {
public class MyService : IMyService {
    private readonly IOrchardServices _services;
    private readonly IMessageManager _messageManager;
    public MyService(IOrchardServices services, IMessageManager messageManager) {
      _services = services;
      _messageManager = messageManager;
    }

    public void DoSomething() {
      var user = _services.WorkContext.CurrentUser.ContentItem.Record;
      var data = new Dictionary<string,string>();
      data.Add("Subject", "Your Subject");
      data.Add("Body", "Hello World");
      _messageManager.Send(user, "MyModuleEmail", "email", data);
    }
  }
}
{% endhighlight %}

*IMessageEventHandler*

The IMessageEventHandler interface is what you use to modify your message before sending. e.g. Setting up the body text, subject, etc. Take a look in the Orchard.Users module to see how they setup the user emails. The implementation is called UserMessagesAlteration. Here is how we could handle the message we triggered in our example above.

{% highlight csharp %}
namespace MyModule.Events {
  public class MyMessageHandler : IMessageEventHandler {
    public void Sending(MessageContext context){
      if (context.MessagePrepared)
        return;
      switch (context.Type) {
        case "MyModuleEmail":
        context.MailMessage.Subject = context.Properties["Subject"];
          context.MailMessage.Body = context.Properties["Body"];
          context.MessagePrepared = true;
        break;
      }
    }

    //we don't care about this right now
    public void Sent(MessageContext context){}
  }
}
{% endhighlight %}

That should do it. Now your message will be sent the Orchard way. Don't forget, you could always do your own thing, but you miss out on some things...

1. Already implemented storage and management of SMTP Settings for sending the email
2. Flexibility to change the messaging channel
3. Consistent event bus propagation for messages

