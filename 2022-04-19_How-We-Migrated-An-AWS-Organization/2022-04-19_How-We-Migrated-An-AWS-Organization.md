# How we migrated an entire AWS Organization to a new one, with no-one noticing!
## Background
Usually when working with AWS, you typically either work with an existing AWS organization that already has guardrails and services in place, or you’re starting from scratch and building a new one. This last one however, is fairly uncommon and being in a situation where you need to establish these things yourself is something even consultants like myself rarely do. However, something that is even more infrequent, is migrating all the accounts from one organization or MSP to another. Amazon has excellent documentation on how to do some of these steps, but they are all typically from individual actions such as settings up a new organization, and not about moving like this.

So how does the process work, and how would you go about migrating from one MSP to another?

In this post, I will discuss how we migrated a production organization from one MSP to another, and try to keep in mind all the minute annoyances that are important to keep in mind when performing such a process.

## Why are we doing this migration?
First, a bit of background on why we’re doing this entire process in the first place.

When the organization I consult for, first started to dip their toes into AWS and cloud production workloads, the parent organization already had an agreement with a MSP that had a good existing security framework and procedures in place. So with limited AWS competency in the organization, it was natural for the company to utilise the help available from the MSP and the guardrails they had in place. However, as the production workloads grew and competency and maturity in the organization grew along with the increased production workloads, the existing framework and procedures to get certain things done felt clunky and slow, resulting in less productive- and happy developers. 

After doing more research, and figuring out the parent organization had reseller agreements with other partners that allowed for more options for us as a company, the decision was made - we were to migrate the entire organization to a new provider. This - as it turns out - is not a simple process…

## Prep Work
In theory, AWS organizations are independent of the accounts, and it should be a simple process to move from one MSP to another. However, in reality, there are a lot of steps you need to complete before, during and after the migration. A couple of key things to remember:

- In our case, the MSP had the responsibility for the accounts, and set the root email and MFA. This meant that because of how AWS accounts work, we needed to create unique email accounts (or aliases) for each account we owned. Not a huge problem, but if you’re using Office 365, each account can only have a very limited number of aliases meaning, we ended up with a huge number of mail accounts that someone needs to manage and keep track of.
- MFA needs to be removed, and then re-applied. For a single account? No big deal. For 50+ accounts, this becomes real tiring, real quick. You also need to establish a system for keeping track of your MFA (or virtual MFA) devices, and which device belongs to which account - then, you need to secure these devices so no-one that should not have access to them, has access.
- ** You actually need to submit a PDF document to AWS where you need to apply to do the migration.** This takes time. You first need to contact AWS support, that sends you the PDF that needs to be signed by a valid signatory from both the outgoing MSP and the new owner, then sent back to AWS. Once submitted, it takes 2-4 weeks for them to process it. This is a step that a lot of MSPs are not aware of, and is vital you don’t forget…
- In most cases, if the outgoing MSP had AWS Security Hub, Config, etc. security policies in place, they will most likely remove them before the move. This means you need to ensure you have a plan in place for how to deal with security in the new organization. **This is a major task!**
- There are billing considerations to be taken as well, an AWS Solutions Advisor will give out specific details here, but it is recommended to do the migration on the last day, or the first day of the month. 

With plans laid out for all these hurdles, we set a date for a couple of days where we would do the migration agreed upon by the outgoing and incoming MSPs, and AWS.
##  Migrating
The migration itself, turns out is a pretty smooth process, as long as you have done the prep work, and the outgoing MSP know what they are doing.

On each account, SCPs were removed, MFA was removed, the old organization was left, then invited to the new one. Once complete, a new MFA device was added to the root user to complete the initial migration.

On average, it took somewhere between 5 - 20 minutes per account we migrated, depending on how many policies were applied to the account and was an incredibly smooth process. The biggest annoyance was reapplying MFA, which is not fun when dealing with this many accounts. 

In the end, we spent about a day and a half migrating all accounts, with zero downtime for any applications, and no users noticing that we even did anything, except that guardrails that had been in place were gone. 

## Post Migration
With the main migration complete, the next step is to establish and rebuild new SCPs, AWS Control Tower, and Guardrails to meet the standards of the new organization. 

Although this is a pretty smooth and straight forward process, we discovered one major hurdle that was pretty annoying. AWS Config uses “Delivery channels” and “Configuration Recorders” to do it’s thing, but didn’t automatically delete these when Config was removed. You need to manually remove them. No big deal, right…? Except that they are applied to each region AWS Config is enabled in, and can only be removed via the AWS CLI or SDK… “No big deal”, you say, AWS Config can just overwrite the existing ones? Nope. They need to be removed. One by one. In each region. Note, you only need to remove the default ones, if you have your own custom, they should be fine.

This can somewhat be automated, but you need to swap credentials between each account, so it isn’t amazing.

Checking if channels and recorders exist for the account
aws configservice describe-configuration-recorders
aws configservice describe-delivery-channels --region eu-north-1

Deleting potential objects - note, you need to do this for EACH region:
aws configservice delete-configuration-recorder --configuration-recorder-name default --region eu-north-1
aws configservice delete-delivery-channel --delivery-channel-name default --region eu-north-1

Once each recorder and delivery channel are deleted, you can enrol the account in AWS Config and Control Tower again!

## Conclusion
Migrating an entire AWS Organization from one MSP to another takes a lot of time. It isn’t necessarily that much physical work, but there is a lot of waiting for each step in the process to complete, and there are a lot of them. 

However, if you do the prep work prior to the migration starting, creating a plan and ensuring everything is in place prior to the move, it turns out it can be a pretty smooth process! 

I highly recommend reaching out to an AWS Solutions Architect prior to such a migration so they can oversee the process and ensure that it goes smoothly, because there are a lot of steps that can be screwed up, and could lead to potential problems. Fortunately, in most cases it should be pretty simple!