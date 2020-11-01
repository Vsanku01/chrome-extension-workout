## Aim: To automate Google Meet using Chrome extension

## Required:
#### 1. Use Key bindings to schedule and share event details
#### 2. Multiple user login and scheduling the meetings

## This repo contains only the web version. This a walk through of the process of developing a extension version of web app

## Steps

#### **Step: 1 **- We should use Google Calendar API to schedule and arrange the meetings cause there is not api available from google meet to create meetings.

#### **Step: 2** - Go to [console.developers.google](https://console.developers.google.com/) and create new Project and add Google Calendar API to the **API & Services** section.

#### **Step: 3** - Generate a key and copy the clientID and Key for further reference

#### **Step: 4 **- Now move to the project folder and create ```manifest.json``` and ```background.html``` and ```background.js```


#### **Step: 5** - Go to the chrome developer Dashbord and register your extension there by uploading the zip of your project folder and generate ```key```. Just a small information this is not the same as the key which you generated in the api console.

#### **Step: 6** - Include them in the ```manifest.json``` and the respective scopes and permissions.

``` json
  "key": "YOUR_KEY",
  "oauth2": {
    "client_id": "YOUR_CLIENT_ID",
    "scopes": [
      "profile email",
      "https://www.googleapis.com/auth/contacts",
      "https://www.googleapis.com/auth/calendar",
      "https://www.googleapis.com/auth/calendar.readonly"
    ]
  },
  "content_security_policy": "script-src 'self' 'unsafe-eval'; object-src 'self'",
  "permissions": [
    "identity",
    "identity.email",
    "storage"
  ]

```


#### **Step 7:** Create Key bindings in ```manifest.json```
```js
  "commands": {
    "create-meeting": {
      "suggested_key": {
        "default": "Alt+N"
      },
      "description": "Create a new meeting"
    },
    "copy-link": {
      "suggested_key": {
        "default": "Alt+C"
      },
      "description": "Copy the current link to clipboard"
    },
    "_execute_browser_action": {
      "suggested_key": {
        "default": "Alt+G"
      }
    }
  },

```


#### **Step 8:** Add Event Listeners in the ```background.js``` to listen for the events from frontend

#### I'm attaching reference docs from this step [https://developer.chrome.com/apps/commands](https://developer.chrome.com/apps/commands)

```js
 chrome.commands.onCommand.addListener(function(command) {
        console.log('Command:', command);
        if(command === 'YOUR_COMMAND'){
          chrome.runtime.sendMessage({
            msg: 'Triggered the Command, You can listen to this message in your popup.js and trigger the associated event'
          })
        }
      });

```


#### **Step 9:** You can use the following snippet to create a meeting, reference [https://developers.google.com/calendar/v3/reference/events](https://developers.google.com/calendar/v3/reference/events)
```js
gapi.client.calendar.events.insert({
  "calendarId": "primary",
  "conferenceDataVersion": 1,
  "resource": {
    "end": {
      "date": "2020-10-24"
    },
    "start": {
      "date": "2020-10-23"
    },
    "conferenceData": {
      "createRequest": {
        "conferenceSolutionKey": {
          "type": "hangoutsMeet"
        },
        "requestId": "some-random-string"
      }
    },
    "summary": "titles are cool"
  }
});

```


#### **Step: 10** - You can send a ``` POST ``` request to the Google calenders api ```  https://www.googleapis.com/calendar/v3/calendars/primary/events?conferenceDataVersion=1  ``` along with the events details for creating a event, further you can store the response in the chrome storage for sharing and copy functionality. Refer to [https://developers.google.com/calendar/v3/reference](https://developers.google.com/calendar/v3/reference) for possible API end points
#### **Step: 11** - Lastly, you can use the meeting link and share. You can add a command to share via social networking apps and share links.



#### 👉 This is just a workout which i did when i was developing. You can send a PR to correct, if i'm wrong 😄




