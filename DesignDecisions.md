# Goal

We want to protect our main branch such that no intrusive commands can be there.
We do not want the opportunity that someone can commit something intrusive.
We will have an admin level and developer level so admin can make changes.

## Approach 1 : Github Actions Workflow

We will have github actions workflow.
When there is a pull request to the main branch or any protected branch
we can stop it before coming in.
For non protected branches we will let them do what they want, but if
they want to merge with main then you have to go through a check and if you fail
you are rejected.
You can only do pull requests to protected branches.
Disable push requests to the branch.
The check will run on github server side.

There will be a configuration file that will have all the intrusive commands you can stop.
Only admin can change that file.

Good for develment workflow so every commit does not have to do a check and cause friction.

Workflows run using the version of the YAML file from the commit that triggered it.


The github workflows files are on the repo and can be manipulated by others.
We must stop that, only the admin can do that.

There are push rules which are only available in organization repos.




### Steps

1. Disable push to protected branches.
2. Have a github workflows

## Approach 2 : Commit Hooks

A pre-commit hook or a pre-receive hook.
Pre-commit hook is a script that runs after running git commit this is ran on client side.
Pre-receive is right after a push and that is ran on server side.

This will run on any branch you are on.
If you do.

git commit --no-verify.

you can bypass these hooks.
Because it is bypassable this does not seem like the best solution.
These scripts are also on the .git directory which is all git metadata.
That metadata is only local on your client side.
Which means that other developers would not receive this.
There is something on github enterprise.