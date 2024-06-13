pipeline{

    agent any

    stages{

        stage('Build Jar'){
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image'){
            steps{
                sh 'docker build -t=mambetov90/artemdockerselenium:latest .'
            }
        }

        stage('Push Image'){
            environment{
                DOCKER_HUB = credentials('dockerhub-creds')
            }
            steps{
                sh 'echo ${DOCKER_HUB_PSW} | docker login -u ${DOCKER_HUB_USR} --password-stdin'
                sh 'docker push mambetov90/artemdockerselenium:latest'
                sh "docker tag mambetov90/artemdockerselenium:latest mambetov90/artemdockerselenium:${env.BUILD_NUMBER}"
                sh "docker push mambetov90/artemdockerselenium:${env.BUILD_NUMBER}"

            }
        }

    }

    post {
        always {
            sh 'docker logout'
        }
    }

}