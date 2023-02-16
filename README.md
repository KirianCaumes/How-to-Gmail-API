# How to Gmail API?

How to acces the Gmail API via your personnal account? 📧

It's not possible to use a service account in this case, so here a simple tutorial to generate an Access Token via OAuth.

## Google Cloud Console

- Go to Google Cloud Console, and create a new project with the name you want: <https://console.cloud.google.com/projectcreate>
- Go to "APIs Library", search for "Gmail API" and click on "Activate": <https://console.cloud.google.com/apis/library/gmail.googleapis.com?project=YOUR_PROJECT>
- Go to "APIs and Services", "Creditentials", "Create Creditentials", "Client ID OAuth". Choose "Application Web", the name you want, add "https://developers.google.com/oauthplayground" as a redirect URI and click on "Create": <https://console.cloud.google.com/apis/credentials/oauthclient?project=YOUR_PROJECT>
- Get the "Client ID" and "Client Secret" from the OAuth created: <https://console.cloud.google.com/apis/credentials/oauthclient/YOUR_OAUTH?project=YOUR_PROJECT>

## OAuth 2.0 Playground

- Go to OAuth 2.0 Playground: <https://developers.google.com/oauthplayground/>
- On the right, click the cog, tick "Use your own OAuth creditentials" and enter your "Client ID" and "Client Secret"
- On the left side, click on "Gmail API v1", choose "https://www.googleapis.com/auth/gmail.readonly" and click the blue button "Authorize APIs"
- Choose your account (same as used in Google Cloud), "Advanced settings", "Acces to YOUR_PROJECT (unsecure)" and "Continue"
- On the left side, click the blue button "Exchange authorization code for tokens"
- You now have the "Refresh token" 🎉

Note: You can manage authorized apps from here: <https://myaccount.google.com/permissions>

## Use the Refresh token

```js
// Simple Node.js example
import { google } from 'googleapis'

const oauth2Client = new google.auth.OAuth2(
    'YOUR_CLIENT_ID',
    'YOUR_CLIENT_SECRET',
)

oauth2Client.setCredentials({
    refresh_token: 'YOUR_REFRESH_TOKEN',
})

const gmail = google.gmail({
    version: 'v1',
    auth: oauth2Client,
})

const messages = await gmail.users.messages.list({
    userId: 'me',
    maxResults: 1,
})

const message = await gmail.users.messages.get({
    userId: 'me',
    id: messages.data.messages[0].id,
})

console.log(message.data.payload)
```
