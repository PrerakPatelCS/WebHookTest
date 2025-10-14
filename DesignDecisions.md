# Goal

We want to protect our main branch or all protected branches such that no intrusive commands can be there.
We do not want the opportunity that someone can commit something intrusive.
We will have an admin level and developer level so admin can make changes.

## Approach 1 : Github Actions Workflow

We will have github actions workflow.
When there is a pull request to the main branch or any protected branch
we can stop it before coming in.
By running this script on server side on Github's servers.

For non protected branches we will let them do what they want, but if
they want to merge to main then you have to go through a check and if you fail
you are rejected.
Pushes directly to main or protected branches is blocked.
The check will run on github server side.

There will be a configuration file that will have all the intrusive commands you can stop.
Only admin can change that file, or we must get approval from the codeowner which will be an admin.

Good for development workflow so every commit does not have to do a check and cause friction.

Workflows run using the version of the YAML file from the commit that triggered it.
Which is why we do not want malicious developers to be able to change this file.

The github workflows files are on the repo and can be manipulated by others.
We must stop that, only the admin can do that.

There are push rules which are only available in organization repos.

In free version we can require an approval to merge to main.
We can have the codereviewer approve the review defined in .github/CODEOWNERS.
For the repo owner you can bypass this approval.
Otherwise it would require at least 1 approver, and if you change anything in .github/ directory
you need approval from the codeowner which we can set to the admin.

Another way is to use file path restrictions.
Restrict any changes to .github/workflows.
And have bypass rules for admins.
This is only in organizations not what this repo is currently.

There is Github Push rules, this is a new feature in Github.
Only available in github Teams and github Enterprise users.
Push rules would allow the admin to make a ruleset where users cannot
edit a specific filepath.
Only Admin should be able to edit files in the .github directory.

Restricted file paths : .github/**/*

### Steps

1. Make a Github ruleset
    a. Target branches
        i. All protected branches.
    b. Branch rules.
        i. Require a pull request before merging.
            a. Required Approvals >= 1
        ii. Require review from codeowners.
    c. Require status checks to pass.
        i. + Add checks -> search for the github actions created.
    d. Block force pushes.
2. Have a Github Actions which will scan the files and stop all Pattern matches from the 
Blocked_Patterns configuration file.
    i. Optimizations possible.
        a. Only scan the files that are changed in the commit.
        b. Parallelize the text search of each file.
3. CodeOwners, add .github/CODEOWNERS file and add this codeowner .github/ @[admin account] 
4. If in Github Teams or Github Enterprise we can add push rules.

https://github.blog/changelog/2024-09-10-push-rules-are-now-generally-available-and-updates-to-custom-properties/

https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets#push-rulesets

```text
Push rulesets are available for the GitHub Team plan in internal and private repositories, and forks of repositories that have push rulesets enabled.
```
    a. Restricted file paths -> + Add file path -> .github/**/*


### Tests

Git push directly to main.
```text
git push
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 12 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 617 bytes | 308.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
remote: error: GH013: Repository rule violations found for refs/heads/main.
remote: Review all repository rules at https://github.com/PrerakPatelCS/WebHookTest/rules?ref=refs%2Fheads%2Fmain
remote:
remote: - Required status check "Scan Code for Intrusive Commands" is expected.
remote:
remote: - Changes must be made through a pull request.
remote:
To https://github.com/PrerakPatelCS/WebHookTest.git
 ! [remote rejected] main -> main (push declined due to repository rule violations)
error: failed to push some refs to 'https://github.com/PrerakPatelCS/WebHookTest.git'
```


 


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