# Stop Intrusive Commands

We want to protect our main branch or all protected branches such that no intrusive commands can be there.
We do not want the opportunity that someone can commit something intrusive.
We will have an admin level and developer level so admin can make changes.

## Steps

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

## Tests

-[x] Git push directly to main.
-[x] Git commit and push directly on the Github UI.
-[ ] Git commit to change files inside of .github directory.
-[x] Git commit with malicious commands.