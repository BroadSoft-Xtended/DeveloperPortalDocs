# Updating Contextual

When the user opens a conversation with one or many users, we will send you the following post request:

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image12.png)

```
POST https://<yourBaseUrl>/:app/timeline

{
  auth: <any> (optional),
  search: <string>, (optional) (this is the string that the user searched)
  context: { (details of the person or persons that you are taking to)
    emails: <array>,
    phoneNumbers: <array>
  }
}

This request will hit your API and you will respond with:

{
  refreshAt: <integer in ms>, (optional)
  items:
    [
     {
       date: <an integer of ms since 1970>,
       title: <string> (first row),
       description: <string> (optional)
       id: <string> (unique id that represents the record),
       url: <string> (open in browser url),
       actions: [
        {
          property: <string> (name of the boolean property of the timeline item),
          iconClass: <string> (name of the icon to be displayed through HubIconFont’s provided css classes),
          color: <string> (css color of the activated action)
        }
      ] (optional)
     },
     {
      /* Similar object */
     }
    ]
}
```

The **Auth** we send you will have been attained by Hub when a user activates your private application from the Hub settings page.

**Search** is the query string that the user has searched for in contextual.

**Context** contains the details of the other users that the target user is talking to.

**Emails** is a list of all user emails in the current user conversation.

**Phone Numbers** is a list of all user phone numbers in the conversation.

**RefreshAt** is the next time (UTC) that we will poll your application for changes. Once that request is sent, you will need to return the next time you want us to check your app for changes and so on.

Timeline is the list of data that you want displayed in contextual. This is an array of objects.

- Date is the date you want displayed. The accepted format is the time since Jan 1 1970 UTC

- Title is the first bolded row of the contextual record

- Description is the one to three lines of details that you may provide for each timeline item.

- Timeline Id is a unique id in your system that represents the timeline item.

- Url is the url the link that is made available so a user can click your timeline item.

- Actions are icons that you want shown as the user hovers over your contextual item. By specifying them in the actions property of the timeline response you can toggle a boolean property of your timeline items. On clicking the icon we will send a PUT request to

https://<hubBaseUrl>/:appName/timeline/:timelineId/:property

With the toggled value of the property in the body of the request.

# Using custom actions

Your timeline is the list of contextual items you want to display. It should look something like this:

```
var timeline = {
    items: [{
      date: <an integer of ms since 1970>,
      title: 'My test record',
      id: 0,
      description: 'My description: user emails you are talking to:' + emails,
      url: 'https://www.google.com',
      actions: [{
        "property": "isRead",
        "title": "Mark as read",
        "titleActive": "Mark as unread",
        "iconClass": "email-read",
        "iconClassActive": "email",
        "type": "toggle"

      }, {
        "property": "deleted",
        "title": "Delete",
        "iconClass": "trash",
        "type": "toggle"
      }, {
        "property": "saveItem", //calls a route nammed saveItem with Id of record
        "type": "list",

        "list": {
          "mode": "toggle",
          "iconClass": "teamOne-saved",
          "title": "Save email to Team-One",
          "header": "Save to Team-One Workspace",
          "value": false,
          "loadItems": {
            "path": "getList",
            "method": "POST",
            "params": {
              "limit": 10
            }
          },
          "searchVisible": true,
          "refreshOnSelect": true
        }
      }, {
        "property": "launchInBrowser",
        "title": "Launch In Browser",
        "type": "link",
        "iconClass": "externallink",
        "url": "https://mail.google.com/mail/?#all/158b6503827f71c2"

      }]
    }]
  };
```

There are three type of contextual actions that we support in the actions seciton:

## type="link"

These actions show a deault link icon for your record and simply link the user to the website you specify.

```
      {
        "property": "launchInBrowser",
        "title": "Launch In Browser",
        "type": "link",
        "iconClass": "externallink",
        "url": "https://mail.google.com/mail/?#all/158b6503827f71c2"
      }
```

## type="toggle"

The toggle route can be much more complex.

```
      {
        "property": "isRead",
        "title": "Mark as read",
        "titleActive": "Mark as unread",
        "iconClass": "email-read",
        "iconClassActive": "email",
        "type": "toggle"
      }
```

### property

This is the route your app will be called with at `/:appName/isRead` This will also send you the Id of the record you specified in the timeline item so you can perform any action you like on that item.

### iconClassActive/iconClass

These are the classes we will show for your icon based on what you return from the property route we call

## type="list"

A list can show the user a searchable list of items that the user can click on.

```
      {
        "property": "saveItem", //calls a route nammed saveItem with Id of record
        "type": "list",
        "list": {
          "mode": "toggle",
          "iconClass": "teamOne-saved",
          "title": "Save email to Team-One",
          "header": "Save to Team-One Workspace",
          "value": false,
          "loadItems": {
            "path": "getList",
            "method": "POST",
            "params": {
              "limit": 10
            }
          },
          "searchVisible": true,
          "refreshOnSelect": true
        }
      }
```
