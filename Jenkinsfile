pipeline {
  environment {
    FUGUE_USER_NAME = credentials("FUGUE_USER_NAME")
    FUGUE_USER_SECRET = credentials("FUGUE_USER_SECRET")
    FUGUE_RBAC_DO_AS = "true"
    FUGUE_LWC_OPTIONS = "true"
    AWS_DEFAULT_REGION = "us-east-1"
    EWC_DNSNAME = "ewc-api.fugue.cloud"
    EWC_USER_NAME = "jonathan@fugue.co"
    EWC_USER_PASS = "asdfasdf"
  }
  agent {
    docker {
      image "225195660222.dkr.ecr.us-east-1.amazonaws.com/fugue/client:latest"
      registryUrl "https://225195660222.dkr.ecr.us-east-1.amazonaws.com/fugue/client"
      registryCredentialsId "ecr:us-east-1:ECS_REPO"
    }
  }
  stages {
    stage("Build") {
      steps {
        sh(script: "zip -r lambda_function.zip lambda_function.py")
      }
    }
    stage("Test") {
      steps {
        sh "lwc LambdaFunction.lw"
        script {
          def cmdStatusCode = sh(script: "fugue status HelloWorldLambda", returnStatus: true)
          if(cmdStatusCode == 0) {
            sh(script: "fugue update HelloWorldLambda LambdaFunction.lw -y --dry-run")
          } else {
            sh(script: "fugue run LambdaFunction.lw -a HelloWorldLambda --dry-run")
          }
        }
      }
    }
    stage("Deploy") {
      when {
        branch "master"
      }
      steps {
        script {
          def cmdStatusCode = sh(script: "fugue status HelloWorldLambda", returnStatus: true)
          if(cmdStatusCode == 0) {
            sh(script: "fugue update HelloWorldLambda LambdaFunction.lw -y")
          } else {
            sh(script: "fugue run LambdaFunction.lw -a HelloWorldLambda")
          }
        }
      }
    }
  }
}
