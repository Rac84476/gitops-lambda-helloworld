node {

  /* Setup our environment */
  withEnv(["AWS_DEFAULT_REGION=us-east-1",
           "FUGUE_RBAC_DO_AS=1",
           "FUGUE_LWC_OPTIONS=true"]) {

    /* Checkout git repo */
    checkout(scm)

    /* Use Amazon ECR repo */
    docker.withRegistry("https://225195660222.dkr.ecr.us-east-1.amazonaws.com/fugue/client", "ecr:us-east-1:ecsrepo" ) {

      /* Pull the fugue client docker container from ECR */
      docker.image("225195660222.dkr.ecr.us-east-1.amazonaws.com/fugue/client:latest").inside {

        /* Set our Fugue credentials */
        withCredentials([string(credentialsId: "FUGUE_USER_NAME", variable: "FUGUE_USER_NAME"),
                         string(credentialsId: "FUGUE_USER_SECRET", variable: "FUGUE_USER_SECRET")]) {

          /* Zip the lambda function */
          stage("Build Serverless App") {
            sh(script: "zip lamdba_function.py")
          }

          /* Test the lambda function */
          stage("Testing Serverless App") {
            def cmdStatusCode = sh(script: "ls", returnStatus: true)
            if(cmdStatusCode == 0) {
              sh(script: "ls", returnStatus: true)
            } else {
              sh(script: "ls", returnStatus: true)
            }
          }

          /* Deploy the lambda function */
          stage("Deploy Serverless App") {
            def cmdStatusCode = sh(script: "ls", returnStatus: true)
            if(cmdStatusCode == 0) {
              sh(script: "ls", returnStatus: true)
            } else {
              sh(script: "ls", returnStatus: true)
            }
          }
        }
      }
    }
  }
}
