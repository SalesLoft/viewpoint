# Viewpoint for Exchange Web Services
[![Build Status](https://travis-ci.org/WinRb/Viewpoint.svg?branch=master)](https://travis-ci.org/WinRb/Viewpoint)
[![Gem Version](https://img.shields.io/gem/v/viewpoint.svg)](https://rubygems.org/gems/viewpoint)
[![License](https://img.shields.io/github/license/WinRb/Viewpoint.svg)](https://github.com/WinRb/Viewpoint/blob/master/LICENSE)
[![Wiki](https://img.shields.io/badge/docs-wiki-lightgrey.svg)](http://github.com/WinRb/Viewpoint/wiki)
[![Documentation](https://img.shields.io/badge/docs-rdoc-lightgrey.svg)](http://www.rubydoc.info/github/WinRb/Viewpoint/frames)

# Viewpoint for Exchange Web Services 1.2.7
http://github.com/zenchild/Viewpoint/wiki

Viewpoint for EWS provides a thin Ruby layer on top of Microsoft Exchange
Web Services(EWS). It also includes a bunch of model classes that add an
additional layer of abstraction on top of EWS for use in implementing
programs with Viewpoint.

BLOG:  http://distributed-frostbite.blogspot.com/

Add me in LinkedIn:  http://www.linkedin.com/in/danwanek

Find me on irc.freenode.net in #ruby-lang (zenChild)

# Features

## Version 1.2.14
1. Fixed a bug where there was an unknown method 

## Version 1.2.13
1. Fixed a bug where an erroneous log statement was placed.

## Version 1.2.12
1. Implemented ability to pass custom headers for the get streaming events call

## Version 1.2.11
1. Implemented new headers for Exchange 2016
2. Implemented a way to pass a customer logger when initializing the gem
3. Added log statements before and after making the request to Exchange with the uniq id of the request

## Version 1.2.10
1. Implemented ErrorExceededFindCountLimit to handle an issue with find_items_between returning ErrorExceededFindCountLimit error

## Version 1.2.9
1. Fixed an issue with XML string contains special charter or invalid elements fails the parsing response

## Version 1.2.8
1. Fixed an issue with too many arguments being passed to the recurring_master_item_id! method in the ews builder

## Version 1.2.7
1. Fixed an issue with too many arguments being passed to the occurrence_item_id! method in the ews builder

## Version 1.2.6
1. Fixed an issue where there was a duplication of methods in the soap builder

## Version 1.2.5
1. Fix an error where the only the first notification in a streaming response's envelope was processed

## Version 1.2.4
1. Fix variable name error

## Version 1.2.3
1. Add support for QueryString (special thank to @Fishy49 https://github.com/Fishy49)
2. Include request body in all error classes for better error handling

## Version 1.2.2
1. Raise `TooManyRequestsError` if HTTP response has error code 429

## Version 1.2.1
1. Enhance response for recurring master so we can get ids for all its occurrence item ids in GetItem response
2. Bug fixed: Generate correct XML body when updating attendees to build proper SOAP request in order to update attendees of a calendar item
3. Removed: Unsupported Ruby Versions from Travis CI

## Version 1.2.0
1. Bug fixed: Use correct body_type when updating a calendar item

## Version 1.1.9 
1. Add SendClientLatencies and ReturnClientRequestId headers

## Version 1.1.8 
1. Support raw HTTP header return from #streaming_subscribe
2. Fixed failing rspec

## Version 1.1.7 
1. Bug fixed for #streaming_subscribe

## Version 1.1.6 
1. Fix typo

## Version 1.1.5 
1. Adding RequestedServerVersion to AutoDiscover #get_user_settings request

## Version 1.1.4 
1. Supports [Exchange Server Affinity](https://docs.microsoft.com/en-us/exchange/client-developer/exchange-web-services/how-to-maintain-affinity-between-group-of-subscriptions-and-mailbox-server#how-do-i-maintain-affinity-by-using-the-ews-managed-api-or-ews)

## Version 1.1.3
1. Supports standard AutoDiscover endpoint
2. Supports customisable http header and cookie in order to support Grouping Subscriptions

## Version 1.1.2
1. Enhance `EwsSoapGetStreamingEventsResponse#error_subscription_ids` to return error_subscription_ids array if any

## Version 1.1.1
Mostly minor bug fix and enhancements

#### Enhancements
1. Make the streaming subscription default timeout to 240 minutes  (same with pull subscription)
2. Use `#streaming_subscribed?` for checking if a streaming subscription is set up

#### Bug fixed
1. `CalendarFolder#items_between` and `GenericFolder#items_between` now truly support option params
 
## New in 1.1.0

* Partially supporting Streaming Notification

```ruby
# Creates a EWS client
ews_client = Viewpoint::EWSClient.new(params["endpoint"], params["username"], params["password"], server_version: params["server_version"])

# Get the folder to streaming_subscribe
folder = ews_client.get_folder(:calendar)
folder.streaming_subscribe

# Creates a streaming connection (HTTPClient::Connection) for receiving data
folder.get_streaming_events(30)
```

## New in 1.0

* SOAP backend is now only dependant on Nokogiri. Before version 1.0 Viewpoint
went through a number of iterations in backends including SOAP4r and Handsoap.
Each of these approaches had major issues so in the end I decided it was
easiest to just build the SOAP messages with Nokogiri since I was using it as
the parser for response messages already.

* Viewpoint is no longer built on a Singleton pattern. The reason it was
previously is because of the Handsoap backend. Handsoap itself uses a
Singleton pattern for connection to the SOAP endpoint so with authentication
I was forced to implement Viewpoint as a Singleton as well. Now with Handsoap
out of the picture this is no longer required. Go crazy ;)

## Enhanced in 1.0

* *Delegate access is supported*
  One thing that was often asked for, but missing from the previous version was delegate access to mailboxes and calendars.  This is now supported via the `act_as` parameter to the `GenericFolder::get_folder` method.

> Inbox example:
  ```inbox = Folder.get_folder(:inbox, opts = {act_as: "user@host.com"})```

> If your user has delegate access to the Inbox for user@host.com this operation will retrieve their inbox and allow you to manipulate it as you would with your own Inbox.

> Calendar example:
  ```calendar = cli.get_folder(:calendar, opts = {act_as: "user@host.com"})```

> If your user has delegate access to the Calendar for user@host.com this operation will retrieve their calendar and allow you to manipulate it as you would with your own Calendar, depending on the permissions the other user has granted you.


* There is also some support for manipulation of delegate access itself via
  the methods `MailboxUser#add_delegate!`, `MailboxUser#update_delegate!`, and
  `MailboxUser#get_delegate_info`.


# Using Viewpoint

The version 1.0 API is quite a departure from the 0.1.x code base. If you have a lot of legacy code and aren't suffering from performance issues, think twice about upgrading. That said, I hope you'll find the new API much clean and more intuitive than previous versions.

I also try and document the code to the base of my ability. Included in that code are links to the official Microsoft EWS documentation that might be helpful when troubleshooting "interesting" issues. You can either generate the documentation yourself with yard or check it out on [rdoc.info](http://rdoc.info/github/zenchild/Viewpoint/frames).

Note the `cli` variable in the setup code directly below. I will use that variable throughout without the setup code.

## The Setup
```ruby
require 'viewpoint'
include Viewpoint::EWS

endpoint = 'https://example.com/ews/Exchange.asmx'
user = 'username'
pass = 'password'

cli = Viewpoint::EWSClient.new endpoint, user, pass
```

There are also various options you can pass to EWSClient.

If you are testing in an environment using a self-signed certificate you can pass a connection parameter to ignore SSL verification by passing `http_opts: {ssl_verify_mode: 0}`.

If you want to target a specific version of Exchange you can pass `server_version: SOAP::ExchangeWebService::VERSION_2010_SP1`. You really shouldn't have to use this unless you know why. If you are interacting with servers which you do not know the version(s) of, an incorrect version may manifest as a `SoapResponseError` or a HTTP 400 `ResponseError`. Note that different versions are particular; for example, `VERSION_2007` may not work against a `VERSION_2007_SP1` system.

## Accessors

There are some basic accessors available on the `Viewpoint::EWSClient` instance. In prior versions these were typically class methods off of the models themselves (ex: `Folder.get_folder(id)`). Now all accessors are available through `EWSClient`.

### Folder Accessors

#### Listing Folders
```ruby
# Find all folders under :msgfolderroot
folders = cli.folders

# Find all folders under Inbox
inbox_folders = cli.folders root: :inbox

# Find all folders under :root and do a Deep search
all_folders = cli.folders root: :root, traversal: :deep
```

#### Finding single folders
```ruby
# If you know the EWS id
cli.get_folder <folder_id>
# ... or if it's a standard folder pass its symbol
cli.get_folder :index
# or by name
cli.get_folder_by_name 'test'
# by name as a subfolder under "Inbox"
cli.get_folder_by_name 'test', parent: :inbox
```

#### Creating/Deleting a folder
```ruby
# make a folder under :msgfolderroot
cli.make_folder 'myfolder'

# make a folder under Inbox
my_folder = cli.make_folder 'My Stuff', parent: :inbox

# make a new Tasks folder
tasks = cli.make_folder 'New Tasks', type: :tasks

# delete a folder
my_folder.delete!
```

### Item Accessors

#### Finding items
```ruby
items = inbox.items

# for today
inbox.todays_items

# since a specific date
sd = Date.iso8601 '2013-01-01'
inbox.items_since sd

# between 2 dates
sd = Date.iso8601 '2013-01-01'
ed = Date.iso8601 '2013-02-01'
inbox.items_between sd, ed
```
### Free/Busy Calendar Accessors

```ruby
# Find when a user is busy
require 'time'
start_time = DateTime.parse("2013-02-19").iso8601
end_time = DateTime.parse("2013-02-20").iso8601
user_free_busy = cli.get_user_availability(['joe.user@exchange.site.com'],
  start_time: start_time,
  end_time:   end_time,
  requested_view: :free_busy)
busy_times = user_free_busy.calendar_event_array

# Parse events from the calendar event array for start/end times and type
busy_times.each { | event |
  cli.event_busy_type(event)
  cli.event_start_time(event)
  cli.event_end_time(event)
}

# Find the user's working hours
user_free_busy.working_hours
```

### Mailbox Accessors
- No examples yet

### Message Accessors
```ruby
cli.send_message subject: "Test", body: "Test", to_recipients: ['test@example.com']

# or
cli.send_message do |m|
  m.subject = "Test"
  m.body    = "Test"
  m.to_recipients << 'test@example.com'
  m.to_recipients << 'test2@example.com'
end

# set :draft => true or use cli.draft_message to only create a draft and not send.
```

## Thanks go out to...

* Handl.it for sponsoring a good portion of the development effort.
  * https://www.handle.com/
* The National Association of REALTORS&reg; for providing a testing account
  and much appreciated input and support.
* The Holmes Group Limited for providing Exchange 2013 test accounts.

## DISCLAIMER:
If you see something that could be done better or would like
to help out in the development of this code please feel free to clone the
'git' repository and send me patches:
`git clone git://github.com/zenchild/Viewpoint.git`
or add an issue on GitHub:
http://github.com/zenchild/Viewpoint/issues

Cheers!
