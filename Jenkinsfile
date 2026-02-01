pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK21'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            when {
                branch 'dev'
            }
            steps {
                sh 'mvn test -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            when {
                branch 'main'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                      mvn sonar:sonar \
                      -Dsonar.projectKey=springboot-restapi \
                      -Dsonar.projectName=SpringBoot-RestAPI
                    '''
                }
            }
        }
    }
}
