pipeline {

  environment {
    REGION = "us-east-1"
    APP_NAME = "tbol-transaction-service"
    AWS_ACCOUT_ID="142199465016"
  }

  parameters {
    string (
      name : 'ECR_REPO_NAME',
      defaultValue: 'tbol-transaction-service',
      description: 'This card will be used for process-automation'
    )

    string (
      name : 'gitBranch',
      defaultValue: 'main',
      description: 'Branch name of the project github repository where the files will be comitted (Use jira card id)'
    )
  }

  agent any

  options{
    buildDiscarder(logRotator(numToKeepStr: '20', artifactNumToKeepStr: '20'))
    skipStagesAfterUnstable()
  }

  stages {

    stage('Build Pipeline Job') {
      steps {
        script{
          buildProject()
        }
      }
    }
  }
}
/**
  Build the project
/**
  Run unit test cases of a project
*/



def buildProject() {
  sh(script:"""
  set -x -v
  """+'''
  SHA=$(git rev-parse HEAD | cut -c1-5)
  export SHA
  eval $(aws ecr get-login --region "$AWS_REGION" --no-include-email)
  tag="${AWS_ACCOUT_ID}.dkr.ecr.us-east-1.amazonaws.com/${ECR_REPO_NAME}:${SHA}"
  echo $tag
  docker build . -t $tag
  $(aws ecr get-login --region ${REGION} --no-include-email)
  docker push $tag
  '''+
  """
  """,
  label:"Building Dockerfile")
}

