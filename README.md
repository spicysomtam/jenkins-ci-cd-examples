# Introduction

A bunch of CI CD Jenkinsfiles deploying into various cloud environments.

# Jenkins setup

I used my own Jenkins instance at home, since I don't want to pay for running an instance continuously in AWS, and I like to have all my Jenkins stuff on this home server. I might include a Cloudformation template to spin one up in AWS (the ECS deployment has a template on the source repo I got the template from). Lets leave it at as a To Do.

You need to install some Jenkins plugins:
 * Pipeline: AWS Steps. This will install dependancies.

You will need to setup the AWS Key and Secret as a credential, which will be passed to the pipelines as a parameter (I used `aws2`).

# Deploying ECS to AWS

Base on [CI CD Jenkins pipelines on AWS getting started.](https://docs.aws.amazon.com/AWSGettingStartedContinuousDeliveryPipeline/latest/GettingStarted/CICD_Jenkins_Pipeline.html). I borrowed the Cloudformation templates from [here](https://github.com/jicowan/hello-world.git); thanks to the author for these (or whoever originally created them).

This uses the AWS Pipeline plugin to deploy a small ECS cluster to AWS.
