pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK21'
    }

    environment {
        SONAR_PROJECT_KEY = 'springboot-restapi'
        SONAR_PROJECT_NAME = 'SpringBoot-RestAPI'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Unit Tests (dev only)') {
            when {
                branch 'dev'
            }
            steps {
                sh 'mvn test -DskipTests=false'
            }
        }

        stage('SonarQube Analysis (main only)') {
            when {
                branch 'main'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                    mvn sonar:sonar \
                      -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                      -Dsonar.projectName=${SONAR_PROJECT_NAME}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline succeeded on branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "❌ Pipeline failed on branch: ${env.BRANCH_NAME}"
        }
    }
}
