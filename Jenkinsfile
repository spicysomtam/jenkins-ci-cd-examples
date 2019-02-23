#!/usr/bin/env groovy

pipeline {

  parameters {
    choice(name: 'action', choices: 'create\ndelete', description: 'Create (or update) or delete the cfn stack.')
    string(name: 'stackname', defaultValue : 'EcsClusterStack', description: "Name of ecs cluster stack.")
    string(name: 'asgSize', defaultValue : '2', description: "ECS ASG default/max size.")
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
          currentBuild.displayName = "#" + env.BUILD_NUMBER + " " + params.action
          def outputs

          switch (params.action) {
            case 'create':
              outputs = cfnUpdate(stack: params.stackname, 
                file:'cfn/ecs-cluster.template', 
                params:["KeyName=${params.keypair}",'EcsCluster=getting-started',"AsgMaxSize=${params.asgSize}"], 
                tags: ['Name=ECS'],
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
              outputs = cfnDelete(stack: params.stackname, 
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

