#!/usr/bin/env groovy

pipeline {

  parameters {
    choice(name: 'action', choices: 'create\ndelete', description: 'Create (or update) or delete the cfn stack.')
    string(name: 'clustername', defaultValue : 'demo', description: "Name of ecs cluster; eg demo.")
    string(name: 'vpcId', defaultValue : '', description: "vpc id to deploy cluster into; leave blank to create a new vpc for the ecs cluster.")
    string(name: 'subnetIds', defaultValue : '', description: "If vpc id specified, comma delimited list of vpc subnet id's.")
    string(name: 'instanceType', defaultValue : 't2.micro', description: "ECS instance type.")
    string(name: 'asgSize', defaultValue : '3', description: "ECS ASG default/max size.")
    string(name: 'keypair', defaultValue : 'myKeypair', description: "EC2 key pair name.")
    string(name: 'credential', defaultValue : 'jenkins', description: "Jenkins credential that provides the AWS access key and secret.")
    string(name: 'region', defaultValue : 'eu-west-1', description: "AWS region.")
  }

  options {
    disableConcurrentBuilds()
    timeout(time: 1, unit: 'HOURS')
    withAWS(credentials: params.credential, region: params.region)
  }

  agent { label 'master' }

  stages {

    stage('Create or delete cfn stack') {
      steps {
        script {
          def outputs
          def clustername = params.clustername
          def stackname = 'EcsCluster' + clustername.capitalize()
          currentBuild.displayName = "#" + env.BUILD_NUMBER + " " + params.action + " " + stackname

          switch (params.action) {
            case 'create':
              outputs = cfnUpdate(stack: stackname, 
                file:'cfn/ecs-cluster.template', 
                params:["KeyName=${params.keypair}",
                  "VpcId=${params.vpcId}",
                  "SubnetIds=${params.subnetIds}",
                  "EcsCluster=${params.clustername}",
                  "EcsInstanceType=${params.instanceType}",
                  "AsgMaxSize=${params.asgSize}"], 
                tags: ["Name=${stackname}"],
                timeoutInMinutes:10, 
                pollInterval:1000)
              println outputs

              // Interestingly this obeys the withAWS directive; but obviously needs awscli installing!
              // sh """
                // aws cloudformation list-stacks
              // """
              break
            case 'delete':
              // To do: You need to scale the number of ecs ec2 instances to 0 before deleting the stack, otherwise it reports an error on delete.
              // The delete does work; but the stack delete shows failure.
              // See this: https://forums.aws.amazon.com/thread.jspa?messageID=663112
              outputs = cfnDelete(stack: stackname, 
                timeoutInMinutes:10, 
                pollInterval:1000)
              println outputs
              break
          }

        }
      }
    }

  }
}

