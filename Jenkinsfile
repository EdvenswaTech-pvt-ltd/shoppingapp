properties([parameters([booleanParam(description: 'Do you want to build?', name: 'build'), choice(choices: ['start', 'stop', 'down'], description: 'do you want to start or stop', name: 'state')])])
pipeline {
    agent any
    triggers {
        githubPush()
    }
    options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
    }

    stages {
        stage('git clone') {
            steps {
                git 'https://github.com/EdvenswaTech-pvt-ltd/shoppingapp.git'
            }
        }
        stage('build') {
            steps {
               sh""" #!/bin/bash
               if [ "$build" = "true" ]; then
               cd src && docker-compose build
               else
               echo "nothing"
               fi
               """
            }
        }
        stage('state') {
            steps {
                sh""" #!/bin/bash
                if [ "$state" = "down" ]; then
                cd src && docker-compose down
                fi
                if [ "$state" = "start" ]; then
                cd src && docker-compose up -d
                elif [ "$state" = "stop" ]; then
                cd src && docker-compose stop
                fi
                """
            }  
        }
        stage("image build") {
            steps {
                
                dir('src/emailservice') {
                sh "docker build -t emailservice2 ."
                sh "pwd"
                }
            }
        }
        stage("push") {
            steps {
                sh"""
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                docker push ajith8790/emailservice2:latest
                """
            }
        }
    }
}
