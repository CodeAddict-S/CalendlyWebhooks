# How to set up a webhook with Calendly

## 1. Create a personal access token from the dashboard

create an account in calendly.com

press the "Generate New Token" button after following the [LINK](https://calendly.com/integrations/api_webhooks)

this will give a bearer token you can use, you must save it!

## 2. Get user information using the bearer token

send a `GET` request to https://api.calendly.com/users/me

include an Authorization header starting like `Bearer xxx`, replace xxx with the bearer token you got from the first step

you will find the response looks something like

```
{
    "resource": {
        "avatar_url": null,
        "created_at": "2025-08-04T12:06:34.916739Z",
        "current_organization": "https://api.calendly.com/organizations/xxx",
        "email": "youremail@gmail.com",
        "locale": "en",
        "name": "Sam Alijev",
        "resource_type": "User",
        "scheduling_url": "https://calendly.com/alijevsam",
        "slug": "alijevsam",
        "time_notation": "24h",
        "timezone": "Asia/Tashkent",
        "updated_at": "2025-08-04T13:19:53.999664Z",
        "uri": "https://api.calendly.com/users/xxx"
    }
}
```

save the `uri` field inside `resource`, it is the USER_UUID_URL

save the `current_organization` field inside `resource`, it is the ORG_UUID_URL

## 3. Register a Webhook with your UUID and Token

send a `POST` request to https://api.calendly.com/webhook_subscriptions, with this body

```
{
    "url": "https://your.domain.com/webhook-path",
    "events": ["invitee.created", "invitee.canceled"],
    "organization": $ORG_UUID_URL,
    "user": $USER_UUID_URL,
    "scope": "user"
}
```
And dont forget to add the `Authorization: Bearer xxx` header

`"scope": "user"` means the events will only be triggered when changes happen to that user only, I will get to that later

you can omit `invitee.created` or `invitee.cancelled` if you don't want to reacive updates on when a new meeting is created or deleted

## 4. Profit

if you have set up a server that listens at `your.domain.com/webhook-path` for updates you should see requests that are sent from Calendly, when someone schedules a meeting with you, take this from now on yourself
