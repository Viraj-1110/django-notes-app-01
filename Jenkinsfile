@Library('jenkins-shared-library@main') _

pipeline {
    agent {
        label 'Jenkins_slave'
    }

    stages {

        stage('Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Viraj-1110/django-notes-app-01.git'

                script {
                    env.IMAGE_TAG = sh(
                        script: 'git rev-parse HEAD',
                        returnStdout: true
                    ).trim()
                }
            }
        }

        stage('Shared Library Build Test') {
            steps {
                dockerBuild('django-notes-app')
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm django-notes-app python manage.py check'
            }
        }

        stage('Docker Push to DockerHub') {
            steps {
                dockerPush(
                    'django-notes-app',
                    'virajmarane1110/django-notes-app',
                    env.IMAGE_TAG
                )
            }
        }

        stage('Deploy on Agent') {
            steps {
                dockerDeploy()
            }
        }
    }
}
