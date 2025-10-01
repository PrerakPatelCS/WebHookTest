# WebHookTest
Test out webhooks

## What is a Webhook

Webhook is a way to notify external services when certain events happen.
When the criteria is met then Github will send a POST request to each of the URLs.
Webhook operates after a push has happened.

## Github Enterprise pre-receive hooks

pre-receive hook can run custom scripts to block pushes.
This is a server side hook, only for Github enterprise.


# Steps

Github repo settings add webhook.
Need a Payload URL which github will send a Post request to.
Specify content type. Usually json.
SSL verification, usually keep enabled.
You need an event to trigger the webhook.

# Delivery and Payload.

```text
Request URL: https://example.com/postreceive
Request method: POST
Accept: */*
Content-Type: application/json
User-Agent: GitHub-Hookshot/e90af57
X-GitHub-Delivery: 31409bf6-9e2f-11f0-8421-40e73f9d1d42
X-GitHub-Event: push
X-GitHub-Hook-ID: 572647827
X-GitHub-Hook-Installation-Target-ID: 1067356266
X-GitHub-Hook-Installation-Target-Type: repository
```

Payload has details of the event.
The event has the sender information, user, commit, etc.

## Github Action Workflow with Ruleset (Server Side)

Github Actions Workflow can validate commits on the server-side and block the push based on results.
Github actions with a protection rule to block merges when a commit has undesirable content.

Make the yml in .github/workflows/ directory.
The yml will tell github how to run it on their servers.

The ruleset.
Settings -> rules -> rulesets.
Make a new ruleset, set the target branch.
Enable restrict updates option.
Require status checks to pass before merging.
Add the validate commit messages job to the list of required status checks.
This stops any pull request or direct push to the branch if the validation workflow fails.

Push the .github/workflows and the workflow will show up on github actions tab.
Make the ruleset and add the workflow action.
It will be a Github Action and it will have the name of the commit.
When it fails, you can check the result in the actions tab.




## Commit hooks (Client Side)

These run on the developers local machine.
Not mandatory, there is a flag to not run it.
THe flag is git commit --no-verify

These hooks are in the .git/hook/ directory.
Make the script executable with chmod +x .git/hooks/commit-msg.

# Test 

intrusive
