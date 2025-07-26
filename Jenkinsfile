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
  - name: kubectl
    image: lachlanevenson/k8s-kubectl:v1.17.2 # use a version that match
    command:
      - cat
    tty: true                         
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
              sh "docker build -t localhost:5000/pythontest:latest ."
              sh "docker push localhost:5000/pythontest:latest"
            }
          }
      }
      stage('Deploy') {
        steps {
          container('kubectl') {
            sh "kubectl apply -f ./kubernetes/deployment.yaml"
            sh "kubectl apply -f ./kubernetes/service.yaml"
          }
        }
      }                     

    }
 }