

Using Twilio with SharePoint to Send and Receive SMS 
============================================================================

SMS Gateway Connector (SMS Via SharePoint List) - CSOM Extensions!

This is the main repository for the SMS Gateway Conenctor Add-in extensions for SharePoint.

<a href="https://timeparity.com/sms-sharepoint-gateway?view=git" rel="SMS Gateway Connector(SMS Via SharePoint List)">![SMS Gateway Connector(SMS Via SharePoint List)](https://timeparity.com/img/sharepoint-gateway-app.gif)</a>

<i class="icon-help-circled"></i> About
-------------

SMS Gateway Connector is SharePoint Add-in available in Microsoft SharePoint Store, that allows easy SMS integration of Twilio  into SharePoint Site. The add-in provisions a SharePoint List which is used to Send SMS request, and another SharePoint List which supports incoming SMS. The Add-in already supports outgoing and incoming sms by now using Twilio API, without a single line of code. This repository intends to achieve a community driven repository of implementations extensions and samples of the Add-in.

•	Official Website: https://timeparity.com/sms-sharepoint-gateway?view=git

•	On SharePoint Store: https://store.office.com/en-001/app.aspx?assetid=WA104380984 
   
> **Note:**

> - Please note this github repository is intended for developers to integrate the add-in programmatically in applications.


![Sending SMS from SharePoint Online / SharePoint 2016.](https://timeparity.com/img/spgway_sendsms_splist_1366x768.png)


Table of contents
-------------

*   [SMS Gateway Connector (SMS Via SharePoint List) - CSOM Extensions!](#)
    *   [About](#about)
    *   [Getting Started](#getting-started)
        *   [Prerequisite](#1-prerequisite)
        *   [Install Library from NUGET](#2-install-library-from-nuget)
        *   [Include Refrences](#3-include-refrences)
    *   [Send SMS to SharePoint User](#send-sms-to-sharepoint-user)
        *   [SMS By Username / Email id](#sms-by-username--email-id)
        *   [SMS By CSOM User Object](#sms-by-csom-user-object)
    *   [Send SMS to Multiple SharePoint Users](#send-sms-to-multiple-sharepoint-users)
        *   [SMS By Username / Email id](#sms-by-username--email-id-1)
        *   [SMS By CSOM UserCollection Object](#sms-by-csom-usercollection-object)
    *   [Send SMS to SharePoint Groups](#send-sms-to-sharepoint-group-)
        *   [SMS By SharePoint Group Name](#sms-by-sharepoint-group-name)
        *   [SMS By SharePoint Group ID](#sms-by-sharepoint-group-id)
        *   [SMS By SharePoint CSOM Group Object](#sms-by-csom-group-object)




Getting Started
-------------
### 1) Prerequisite

**Install SMS Gateway Connector(SMS via SharePoint List) Add-in**
> - Install the add-in from Microsoft SharePoint Store, installation steps [here](https://timeparity.com/Content/timeparity-sms-gateway-installation-guide.pdf)
> - Add-in Name: **SMS Gateway Connector(SMS via SharePoint List)**
> - Office Store asset ID : [**WA104380984**](https://store.office.com/en-001/app.aspx?assetid=WA104380984 )

> ![SMS For Office SharePoint Add-in](https://az158878.vo.msecnd.net/marketing/Partner_21474845989/Product_42949680853/Asset_e72d6535-7ec3-4682-bc9f-d699c0a84573/SMSGatewayIcon300x300.png)



### 2) Install Library from NUGET

The Library is available in on Nuget, please get the latest version depending on your SharePoint Environment:

>**SharePoint Online**

Nuget:  [SharePoint Online SMS Integration](https://www.nuget.org/packages/TimeParity.SharePointOnline.SMS)
```powershell
PM> Install-Package TimeParity.SharePointOnline.SMS
```


>**SharePoint 2016**

Nuget:  [SharePoint 2016 SMS Integration](https://www.nuget.org/packages/TimeParity.SharePoint2016.SMS)
```powershell
PM> Install-Package TimeParity.SharePoint2016.SMS
```

### 3) Include refrences

Include the following refrences in your .Net Application:

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
using Microsoft.SharePoint.Client;
```

 <i class="icon-user"></i>Send SMS to SharePoint User
-------------

Once you have included the [references](#3-include-refrences) , you have the following options to chose from for sending SMS to your SharePoint Site User

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
using Microsoft.SharePoint.Client;
```
#### <i class="icon-user"></i>SMS By Username / Email id

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message  
/// to user of provided user id (loginame or email id)
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="loginNameOrEmail">loginname or email id of the user</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToUser(this ClientContext context, bool customGateway, string loginNameOrEmail, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

// Replace with a valid user email id from your SharePoint site
string emailid = "bob@foobar.onmicrosoft.com";

// Replace with a valid username from your SharePoint site
string username = "i:0#.f|membership|steve@foobar.onmicrosoft.com";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
    SMSRequestResult result = clientcontext.SendSMSToUser(true, emailid , smscontent);
    Console.WriteLine("Requested by email id, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
    
    SMSRequestResult result2 = clientcontext.SendSMSToUser(true, username , smscontent);
    Console.WriteLine("Requested by username, Result : {0} , Message: {1} "
    , result2.status.ToString(), result2.status_message);
}
```
#### <i class="icon-user"></i>SMS By CSOM User Object

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to user   
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="user">SharePoint User Object</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToUser(this ClientContext context, bool customGateway, User user, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
   // Get current logged in user
    clientContext.Load(clientContext.Web);
    clientContext.ExecuteQueryRetry();    
    User user = clientcontext.Web.CurrentUser;
    
    //Send SMS to User
    SMSRequestResult result = clientcontext.SendSMSToUser(true, user , smscontent);
    Console.WriteLine("Requested user object, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
     
}
```
 <i class="icon-user"></i>Send SMS to Multiple SharePoint Users
-------------

Once you have included the [references](#3-include-refrences) , you have the following options to chose from for sending SMS to your SharePoint Site Users

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
using Microsoft.SharePoint.Client;
```
#### <i class="icon-user"></i>SMS By Username / Email id

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message  
/// to users with provided user id(s) - loginame(s) or email id(s)
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="loginNameOrEmail">A list/Array of loginname or email id of the users</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
///  recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToMultipleUsers(this ClientContext context, bool customGateway, 
IEnumerable<string> loginNameOrEmail, string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

// Replace with a valid user email id from your SharePoint site
string emailid = "bob@foobar.onmicrosoft.com";

// Replace with a valid username from your SharePoint site
string username = "i:0#.f|membership|steve@foobar.onmicrosoft.com";

// An Array of users
string[] userArray = { emailid, username };

// List of users
List<string> userList = userArray.ToList();

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
    // Send SMS to an array of users, 
    // create a new request for each user (batchsize = 1)   
    SMSRequestResult result = clientcontext.SendSMSToMultipleUsers(true, userArray , smscontent, 1);
    Console.WriteLine("Requested by array");
    Console.WriteLine("Result : " + result.status.ToString());
    Console.WriteLine("Result Message: " + result.status_message);
    
    // Send SMS to a List of users, 
    // create a single request for every 25 users  (batchsize default value is 25)
    SMSRequestResult result2 = clientcontext.SendSMSToMultipleUsers(true, userList , smscontent);
    Console.WriteLine("Requested by list");
    Console.WriteLine("Result : " + result2.status.ToString());
    Console.WriteLine("Result Message: " + result2.status_message);
}
```
#### <i class="icon-user"></i>SMS By CSOM UserCollection Object

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to user   
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="users">SharePoint UserCollection Object</param>   
/// <param name="message">SMS Message content</param>  
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
/// recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToMultipleUsers(this ClientContext context, bool customGateway, UserCollection users, 
string message, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
   // Get users from default memebers group
    clientContext.Load(clientContext.Web);
    clientContext.Load(clientContext.Web.AssociatedMemberGroup, i => i.Users);
    clientContext.ExecuteQueryRetry(); 
    
    //CSOM UserCollection object
    UserCollection users = clientContext.Web.AssociatedMemberGroup.Users;
    
    //Send SMS to Users
    SMSRequestResult result = clientcontext.SendSMSToMultipleUsers(true, users , smscontent);
    Console.WriteLine("Requested by usercollection object");
    Console.WriteLine("Result : " + result.status.ToString());
    Console.WriteLine("Result Message: " + result.status_message);
     
}
```

 <i class="icon-user"></i>Send SMS to SharePoint Group 
-------------

Once you have included the [references](#3-include-refrences) , you have the following options to chose from for sending SMS to users of your SharePoint Group

```c#
using TimeParity.SharePoint.SMS;
using TimeParity.SharePoint.SMS.Extensions;
using Microsoft.SharePoint.Client;
```
#### <i class="icon-user"></i>SMS By SharePoint Group Name

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to users of a SharePoint Group
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="groupTitle">SharePoint Group Title</param>   
/// <param name="message">SMS Message content</param>     
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
///  recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param> 

public static SMSRequestResult SendSMSToSharePointGroupUsers(this ClientContext context, bool customGateway, string groupTitle,
string message, int batchSize = 25, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

// Replace with a valid SharePoint Group Namme
string groupTitle = "bob@foobar.onmicrosoft.com";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
    //Send SMS to Group Users
    SMSRequestResult result = clientcontext.SendSMSToSharePointGroupUsers(true, groupTitle , smscontent);
    
    Console.WriteLine("Requested by group name, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
    
}
```
#### <i class="icon-user"></i>SMS By SharePoint Group Id

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to users of a SharePoint Group
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="groupID">SharePoint Group Id</param>   
/// <param name="message">SMS Message content</param>     
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
/// recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToSharePointGroupUsers(this ClientContext context, bool customGateway, int groupID, 
string message, int batchSize = 25, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

// Replace with a valid SharePoint Group Id
int groupID = 4;


using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
    //Send SMS to Group Users
    SMSRequestResult result = clientcontext.SendSMSToSharePointGroupUsers(true, groupID , smscontent);
    
    Console.WriteLine("Requested by group id, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
}
```
#### <i class="icon-user"></i>SMS By CSOM Group Object

>**Extension Definiton**

```c#
/// <summary>
/// Creates entry in SMS Request List for sending the message to users of a SharePoint Group
/// </summary>
/// <param name="customGateway">Boolean Value to be set as true while using Twilio / Plivo</param>   
/// <param name="group">SharePoint Group</param>   
/// <param name="message">SMS Message content</param>     
/// <param name="batchSize">Optional limit to specify maximum recipients per request,
///  recommended value is 25</param>  
/// <param name="title">Optional title to be set for the request, default is "Custom"</param>  

public static SMSRequestResult SendSMSToSharePointGroupUsers(this ClientContext context, bool customGateway, Group group, string message, int batchSize = 25, string title = "Custom")
```

>**Usage**

```c#

// Office Dev PNP Authentication Manager
var am = new OfficeDevPnP.Core.AuthenticationManager();

// Url of the site where add-in is already installed
string siteUrl = "https://foobar.sharepoint.com";

// The content of your SMS message
string smscontent = "A message from SharePoint";

using (ClientContext clientcontext = am.GetWebLoginClientContext(siteurl))
{ 
   // Get default owner group
    clientContext.Load(clientContext.Web);
    clientContext.ExecuteQueryRetry();   
    
    Group group = clientcontext.Web.AssociatedOwnerGroup;
    
    //Send SMS to Owner Group Users
    SMSRequestResult result = clientcontext.SendSMSToSharePointGroupUsers(true, group , smscontent);
    
    Console.WriteLine("Requested by CSOM Group object, Result : {0} , Message: {1} "
    , result.status.ToString(), result.status_message);
     
}
```
