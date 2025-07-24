pipeline {
  agent {
    kubernetes {
      label 'jenkins-python'
      defaultContainer 'python'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: jenkins-python
spec:
  containers:
  - name: python
    image: python:3.10-slim-bullseye
    command: ['cat']
    tty: true
  - name: jnlp
    image: jenkins/inbound-agent:latest
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
"""
    }
  }

  stages {
    stage('Run Tests') {
      steps {
        container('python') {
          sh 'pip install -r requirements.txt'
          sh 'python test.py'
        }
      }
    }
  }
}
