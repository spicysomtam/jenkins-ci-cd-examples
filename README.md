# Introduction

Deploy ecs via a cloudformation template/stack. I considered using terraform, but the example in [terraform-aws-provider](https://github.com/terraform-providers/terraform-provider-aws) uses CoreOs images and a non standard way of deploying, and I did not want to spend too much time adapting that.

Base on [CI CD Jenkins pipelines on AWS getting started](https://docs.aws.amazon.com/AWSGettingStartedContinuousDeliveryPipeline/latest/GettingStarted/CICD_Jenkins_Pipeline.html). I borrowed the Cloudformation templates from [here](https://github.com/jicowan/hello-world.git); thanks to the author for these (or whoever originally created them). Maybe they are just sample templates?

I use my own Jenkins instance at home, since I don't want to pay for running an instance continuously in AWS, and I like to have all my Jenkins pielines ready to run when I need them. Click a button and wait for whatever I need to be deployed without having to waste time working out how to deploy it. 

Pipelines like this also allows me to spin up infrastructure quickly when I need to develop/test/run examples, without the infrastructure running all the time, which costs money.

# Jenkins setup

The pipeline is manually setup on your Jenkins instance, and runs a Jenkinsfile from this code checkout.

You need to install some Jenkins plugins:
 * Pipeline: AWS Steps. This will install dependancies.

You will need to setup the AWS Key and Secret as a credential, which will be passed to the pipelines as a parameter (I called it `jenkins`).

I would also recommend installing the `awscli`; same version as the api installed via the AWS Steps plugin; or should I say try and keep the versions in sync. Sometimes you may need to work around issues and use the cli. Interestingly the cli obay the pipeline `withAWS` (eg credentials, region, etc).
