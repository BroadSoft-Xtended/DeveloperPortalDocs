# Updating Contextual Data

When the user opens a conversation with one or many users, we will send you the following post request:

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image12.png)

```
POST https://<yourBaseUrl>/v1/:app/timeline

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
       date: <Time since Jan 1 1970 UTC>,
       title: <string> (first row),
       description: <string> (optional)
       timelineId: <string> (unique id that represents the record),
       webLink: <string> (open in browser url),
       actions: [
        {
          property: <string> (name of the boolean property of the timeline item),
          iconClass: <string> (name of the icon to be displayed through HubIconFont’s provided css classes),
          Color: <string> (css color of the activated action)
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

- Sub title is the optional second row of the contextual record

- Description is the one to three lines of details that you may provide for each timeline item.

- Timeline Id is a unique id in your system that represents the timeline item.

- WebLink is the url the link that is made available so a user can click your timeline item.

- Actions are icons that you want shown as the user hovers over your contextual item. By specifying them in the actions property of the timeline response you can toggle a boolean property of your timeline items. On clicking the icon we will send a PUT request to

https://

<baseurl>/v1/:app/:username/timeline/:timelineId/:property</baseurl>

With the toggled value of the property in the body of the request.
