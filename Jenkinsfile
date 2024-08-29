pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf my-code'
            sh 'git clone https://github.com/surya-personal-repos/my-code.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'docker build -t jayasurya/new:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push jayasurya/new:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.0.16.115:2375 stop webapp1 || true'
            sh    'docker -H tcp://10.0.16.115:2375 run --rm -dit --name webapp1  -p 8000:80 jayasurya/new:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.0.16.115:8000'
          }
        }

    }
}
