# Introduction

A bunch of CI CD Jenkinsfiles deploying into various cloud environments.

The pipelines are manually setup on your Jenkins instance, but run Jenkinsfiles and code from checked out git repos (like this).

# Jenkins setup

I used my own Jenkins instance at home, since I don't want to pay for running an instance continuously in AWS, and I like to have all my Jenkins setups on this home server. I included a Cloudformation template in the cfn dir to spin one up in AWS. Lets leave it at as a To Do/work in progress?

You need to install some Jenkins plugins:
 * Pipeline: AWS Steps. This will install dependancies.

You will need to setup the AWS Key and Secret as a credential, which will be passed to the pipelines as a parameter (I used `aws2`).

I would also recommend installing the `awscli`; same version as the api installed via the AWS Steps plugin; or should I say try and keep the versions in sync. Sometimes you may need to work around issues and use the cli. Interestingly the cli obay the pipeline `withAWS` (eg credentials, region, etc).

# Deploying ECS to AWS

Base on [CI CD Jenkins pipelines on AWS getting started](https://docs.aws.amazon.com/AWSGettingStartedContinuousDeliveryPipeline/latest/GettingStarted/CICD_Jenkins_Pipeline.html). I borrowed the Cloudformation templates from [here](https://github.com/jicowan/hello-world.git); thanks to the author for these (or whoever originally created them). Maybe they are just sample templates?

This uses the AWS Pipeline plugin to deploy a small ECS cluster to AWS.
