pipeline{
    agent {label "slave3"}
    environment{
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')
    }
    stages{
        stage('checkout scm'){
            steps{
               git branch: 'main', changelog: false, credentialsId: 'github_cred', poll: false, url: 'https://github.com/VINUTHKUMARMR/Docker_tomcat.git'
            }
        }
        stage('build image'){
            steps{
                sh '''
                  
                  docker build -t daemon/tomcat:$BUILD_NUMBER .
                  docker tag daemon/tomcat:$BUILD_NUMBER vinuthkumarmr/docker_tomcat:$BUILD_NUMBER
                  docker run -it -d --name newcon4 -p 8091:8080 vinuthkumarmr/docker_tomcat:$BUILD_NUMBER
                  
                  '''
            }
        }
        stage('loginto-hub'){
            steps{
                 sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image to hub'){
            steps{
                 sh 'docker push vinuthkumarmr/docker_tomcat:$BUILD_NUMBER'
            }
        }
    }
    post {
        always{
            sh 'docker logout'
        }
    }
}
