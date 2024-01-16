This repository has been setup for Microsoft personal to play with an demo GHAzDO. 

## Resources ##
 - [GHAzDO pitch](https://microsoft.sharepoint.com/teams/GithubSales/_layouts/15/search.aspx/siteall?q=GHAZDO%20Pitch%20Deck) deck on [ghsales](aka.ms/ghsales)
 - [GHAzDO intro demo video](https://www.youtube.com/watch?v=cTkUhKkMD_c), covering why a tool like this is needed and how to set it up. This video is usually hidden behind a form to collect leads so do not spread it to far.
 - [ADO roadmap](https://aka.ms/azdo-roadmap)
 - [PR Gating Script](https://gh.io/GHAzDO_pr_gating). PR Gating is added to this repo via scripts and configurations. The product team plans to add this feature at a later state, the current setup is a temporary workaround. 
 - [GHAzDO documentation](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features).


## Preparations ##

Please review the GHAzDO pitch deck and the intro demo video. The pitch deck is useful if you need to explain the why of Advanced Security. The demo is a good next step after the customer understands the need. 
Advanced customers might want to dig deeper into the GHAzDO security capabilities. For larger customers, you will get support from the GitHub account team in these discussions if you reach out. 

Create a new ADO PAT that can be used for showing off push protection
[Setup a new ADO PAT](https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Windows#create-a-pat). 
Give it minimal access rights. Copy the key value. Revoke the new PAT right away.

## Demo for the customer ##

Show how GHAzDO is enabled on the repo in the ADO UI. This is a competitive advantage, most of our competitors have clunky setups and extra installs for this. You can also switch over to the security tab of the repo and who the GHAzDO security roles and how they are integrated with ADO.

Talk about what happens when GHAzDO is enabled. How we will 'start a background process' to scan for forgotten secrets in the codebase. Leaked credentials is one of the main attack vectors and most of our competitors does not do it. 
Show Push Protection. I usually setup a new file (config.txt) and add ADO_PAT = [the key from your pre prep]. This is obviously a simplification of a more realistic scenario but it usually gets the point across. Try to commit the new file and notice the push protection UI. Show how you can change just 1 char of the PAT and the push will not be blocked. This shows how we can keep our false positive rate low.

Switch over to the GHAzDO report UI and show the secrets that was added to the project before push protection was enabled. If asked, show the secret providers we work with from the GHAzDO documentation.

Talk about how secret scanning is the feature that can be enabled with just a click of a checkbox. For dependency scanning and code scanning, a pipelines needs to be extended. Show the GHAS pipeline and note the CodeQL and dependency scanning tasks. 

Show in the GHAS pipeline build logs the result of a dependency scan. Show the number of direct and transient dependencies. We will check all of these libraries against the GitHub advisory database. Any libraries used with known vulnerabilities will be flagged and show in the GHAzDO UI. Show the dependency alerts. Dig into one vulnerability. Talk about the details and that this was a transient dependency, the UI shows the root dependency that brought it in.

Switch to code scanning. Talk about how CodeQL is a two step process, of first building a Database describing the dataflow of the application and that we then run queries against this database. Show the results of the code scanning. Dig into one vulnerability and show the details. Jump into the code that is referenced, I usually pick a SQL injection. Show the code and explain the issue, mention that we know in this instance that the SQL query is user controlled, that is, the tool has detected that some parts of the SQL query comes unfiltered from user input. Again, since we can follow the data from source to sink we can keep our false positive rate low. 

PR gating come up in most discussions. That is, the ability to block new code going into the main branch if it adds any new vulnerabilities. If this come up, show the PR that is there and blocked. Show how a branch policy has been set on the main branch. Show the result of the PR - how it has been annotated with information about the vulnerabilities. Jump into the details referenced in the PR annotation and show how the scan has happened on the PR branch. 

