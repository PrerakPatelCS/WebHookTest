# WebHookTest
Test out webhooks

## What is a Webhook

Webhook is a way to notify external services when certain events happen.
When the criteria is met then Github will send a POST request to each of the URLs.

# Steps

Github repo settings add webhook.
Need a Payload URL which github will send a Post request to.
Specify content type. Usually json.
SSL verification, usually keep enabled.
You need an event to trigger the webhook.

