# How we migrated an AWS Organization
## Background
Usually when working with AWS, you deal with an already existing organization with pre-existing guardrails or services in place. Being in a situation where you need to establish these things yourself is fairly uncommon, even for consultants, but something that is even rarer is to migrate a number of accounts from one organization or MSP to another. Amazon has some excellent documentation on most steps, but they are all typically from individual actions such as settings up a new organization, and not about a move process.

So how does the process work, and how would you go about migrating from one MSP to another?

In this post, I will discuss how we migrated a production organization from one MSP to another, and try to keep in mind all the minute annoyances that are important to keep in mind when performing such a process.

## Why are we doing this migration?
First, a bit of background on why we’re doing this entire process in the first place.

When the organization first started to dip their toes into AWS and cloud production workloads, the parent organization already had an agreement with a MSP that had a good existing security framework and procedures in place, and with limited AWS competency in the organization, it was a natural move to use the existing resources. However, as the production workloads grew and competency and maturity in the organization grew along with the increased production workloads, the existing framework and procedures to get certain things done felt clunky and slow, resulting in less productive- and happy developers. 



