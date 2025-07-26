pipeline {
    agent {
        kubernetes {
            inheritFrom 'default'
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.7
    command:
    - cat
    tty: true
  - name: docker
    image: docker
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: /var/run/docker.sock
        name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
"""
        }
    }
  
  triggers {
    pollSCM('* * * * *')
  }
   
    stages {
        stage('Test python') {
            steps {
                container('python') {
                    sh "python --version"
                    sh "pip install -r requirements.txt"
                    sh "python test.py"
                }
            }
        }
       
       stage('Build image') {
        steps {
          container('docker') {
            sh "docker build -t registry.jenkins.svc.cluster.local:5000/pythontest:latest ."
            sh "docker push registry.jenkins.svc.cluster.local:5000/pythontest:latest"
          }
        }
        }

    }
 }