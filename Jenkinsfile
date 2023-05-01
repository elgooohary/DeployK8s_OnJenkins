pipeline {

  agent any

  stages {

properties([parameters([choice(choices: ['main', 'feature_1'], description: 'Select desired branch to build', name: 'branches')])])

node{

stage('checkout from github') {

echo "checking-out from branch ${params.branches}"

git url: 'https://github.com/elgooohary/DeployK8s_OnJenkins', branch: "${params.branches}"

}

}
    stage('Checkout Source') {
      steps {
        git 'https://github.com/elgooohary/DeployK8s_OnJenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
