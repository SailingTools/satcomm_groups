# Group chat with IridiumGo!, Satphone and/or email 

This is a serverless message aggregation service for the Iridium Go.  It allows users to subscript to message groups.  Once users are subscribed then they will receive all of the messages that are sent to that group, thus enabling many-to-many "group" type chat over the IridiumGo.  This serverless function controls the sub/unsub of users to particular groups and will distribute messages out to everyone in a group.

## Key required features:

  * Long messages that are sent are broken down into multiple smaller messages (max 160 characters)
  * Users can subscribe/unsubscribe from groups
  * Users can list groups that they are a member of (low priority)
  * Groups can be marked as public and then are searchable and can be joined by anyone (low priority)


## Getting started (Quickstart)

Follow these steps to get started:

1. From your IridiumGo create a new SMS message to "group@svlogbook.com" with the content "CREATE_GROUP My Group Name" (this can also be done from any email address such as a Gmail address).
  * This creates a "group" (chat group) and you should get an response back saying something like: "You created this group (4ea80ddc). Others can join by sending a message to group+4ea80ddc@svlogbook.com with the word SUBSCRIBE"

2. From another IridumGo (or from another gmail/email account, etc. ) send a message (or email) to the group address created above (group+4ea80ddc@svlogbook.com) with the word "SUBSCRIBE" in the body
  * Again you should get an email response back saying: "You have subscribed to the group (4ea80ddc). To unsubscribe send an email to group+4ea80ddc@svlogbook.com with the word UNSUBSCRIBE"

3. Now there are two addresses in the group.  You can check the users that are in the group at any time by sending an email to the group address (group+4ea80ddc@svlogbook.com) with the body "LIST_USERS".

4. Now from either of the addresses in the group send a message to the group-address (group+4ea80ddc@svlogbook.com) with any arbitrary content (something like: Here is a test message).
  * You should find that that message is relayed to the other addresses in the "group" prefixed with a small "handle". So for example from my personal email address (Mark Pitman <pitman.mw@gmail.com>) the handle is worked out to be "MP".  So messages from that user/address will be prefixed with "MP:" so that other users reading the messages streaming in can identify who wrote which message.
  * Because all of the messages are sent through the group-address (both income and outgoing) then you SHOULD hopefully find that all the messages from that group come under the same chat-thread within the IridiumGo SMS app (however I cant do anything about the fact that that app messes up the ordering unfortunately).

### Long messages
If a message is longer than 160 characters (including the handle).  Then the message is split into multiple 160 character messages and distributed like that.

### Changing your handle: 
Each email address/user on the system has a short "handle" that is used to identify the message has come from them.  
  * We try to kind of intelligently set the handle from the users email address (so for example Mark Pitman <pitman.mw@gmail.com> takes the full-name and just uses the initials. So the handle will be "MP".  If there is no name-information in the email address as above, then I try to get the first 6 characters from the email address, which would be your callsign on some email addresses.
  * If you want to change your handle then just send an email to group@svlogbook.com with the body "SET_HANDLE <new>"
  * So for example you would probably want to change your handle with "SET_HANDLE ZA".  Then all messages from you will be prefixed with ZA:

### Checking which groups you are subscribed to
If you are subscribed to many groups it might be handy to get a list of what groups you are subscribed to.  You can do this by sending a message to "group@svlogbook.com" with the body "LIST_GROUPS"

### Setting a group name: 
You would notice if you did the LIST_GROUPS above command above that it lists out the group id numbers with :None after each.  That is because names have not been assigned to the group.  You can set a more human-readable name for the group by sending the command "SET_GROUP_NAME <name>" to the group-address.  So for example I could email "group+4ea80ddc@svlogbook.com" with the command "SET_GROUP_NAME Pacific ocean crossing 2023" and the name "Pacific ocean crossing 2023" will be assigned to that group.  Now when you LIST_GROUPS command then it will display that group as: "4ea80ddc:Pacific ocean crossing 2023"

### Getting Help: 
If you forget these instructions then you can just send the message "HELP" to either the root-level (group@svlogbook.com) or to a group-level address (group+4ea80ddc@svlogbook.com).  This will send you back a list of commands that you can execute at each level.


## General architecture
The system uses a "serverless" architecture on the backend to maximise stability, scalability and uptime.  It should be able to handle a large volume of traffic and stay online reliably.
