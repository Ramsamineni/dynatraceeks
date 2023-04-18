pipeline {
  agent any
  stages {
    stage('Checkout Git Repository') {
      steps {
          git branch: 'ram', credentialsId: 'ram-GitHub', url: 'https://github.com/cerebrone-ai/Terraform.git'
        }
      }
    }
    stage('Deploy Dynatrace Operator') {
      steps {
          sh "kubectl config use-context Dynatrace-ram"
          sh "kubectl create namespace dynatrace"
          sh "kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v0.10.4/kubernetes.yaml"
          sh "kubectl -n dynatrace wait pod --for=condition=ready --selector=app.kubernetes.io/name=dynatrace-operator,app.kubernetes.io/component=webhook --timeout=300s"
          sh "git clone -b ram https://github.com/cerebrone-ai/Terraform.git repo"
          sh "cd repo/dynatrace"
          sh "git checkout ram"
          sh "kubectl apply -f dynakube.yaml"
        }
      }
    }
  }
