pipeline {
    agent {
        kubernetes {
            label 'jenkins-agent'
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
    image: python:3.10
    command:
    - cat
    tty: true
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
