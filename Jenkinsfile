pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build & Unit Test') {
            steps {
                sh 'mvn clean test -DskipTests'
            }
        }
        stage('Auto Merge Dev → Main') {
            when {
                branch 'dev'
            }
            steps {
                withCredentials([usernamePassword(
            credentialsId: 'GitHub-PAT',
            usernameVariable: 'GIT_USER',
            passwordVariable: 'GIT_TOKEN'
        )]) {
            sh '''
              git config user.name "jenkins-bot"
              git config user.email "jenkins@local"

              git fetch origin main
              git checkout -B main FETCH_HEAD
              git merge origin/dev

              git push https://${GIT_USER}:${GIT_TOKEN}@github.com/rounak13062001/SpringBoot-RestAPI.git main
            '''
        }
    }
}

    }
}
