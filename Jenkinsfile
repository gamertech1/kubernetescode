pipeline{
    agent any
    stages{
        stage("checkout scm"){
            steps{
            echo "pull code from scm"
        }
    }
        stage("build image"){
            steps{
            echo "building image"
            sh "docker build -t my-app ."
        }
    }
        stage("push image to dockerhub"){
            steps{
            echo "pushing image to dockerhub"
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerhubpass', usernameVariable: 'dockerhubuser')]) {
            sh "docker tag my-app ${env.dockerhubuser}/my-app:v1"
            sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
            sh "docker push ${env.dockerhubuser}/my-app:${env.BUILD_NUMBER}"
 }
      }
            
        }
        stage("kubernetes manifest"){
         steps{
            echo "trigger kubernetes manifest"
            build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: 'env.BUILD_NUMBER')]
        }
        }
    }
}
