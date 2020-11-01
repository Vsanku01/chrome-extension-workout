## Aim: To automate Google Meet using Chrome extension

## Required:
#### 1. Use Key bindings to schedule and share event details
#### 2. Multiple user login and scheduling the meetings

#### This repo contains only the web version. Although I will give a walk through the process of developing a extension

## Steps

#### Step: 1 - We should use Google Calendar API to schedule and arrange the meetings cause there is not api available from google meet to create meetings.

#### Step: 2 - Go to [console.developers.google](https://console.developers.google.com/) and create new Project and add Google Calendar API to the **API & Services** section.

#### Step: 3 - Generate a key and copy the clientID and Key for further reference

#### Step: 4 - Now move to the project folder and create ```manifest.json``` and ```background.html``` and ```background.js```


#### Step: 5 - Go to the chrome developer Dashbord and register your extension there by uploading the zip of your project folder and generate ```key```. Just a small information this is not the same as the key which you generated in the api console.

#### Step: 6 - Include them in the ```manifest.json``` and the respective scopes and permissions.

```
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


#### Step 7: Create Key bindings in ```manifest.json```
```
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


#### Step 8: Add Event Listeners in the ```background.js``` to listen for the events from frontend

#### I'm attaching reference docs from this step [https://developer.chrome.com/apps/commands](https://developer.chrome.com/apps/commands)

```
 chrome.commands.onCommand.addListener(function(command) {
        console.log('Command:', command);
      });

```



