pipeline {
    agent any
    tools {
        maven "Maven 3.9.2"
    }
    stages {
        stage('Pull Code from Git Hub') {
            steps {
                // Pull the code form SCM Repo ---> git Hub
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/Sankeerth-Chillamcharla/JavaApp.git'
            }
        }
        stage('Code Cleaning') {
            steps {
                sh "mvn clean"
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=javaapp -Dsonar.projectName='javaapp'"
                }
            }
        }
        stage('Quality Gate') {
            steps {
                sleep 60
                script {
                def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                           }
                        else {
                            echo "Quality Gate passed: ${qg.status}"             }    
                         }
            }
        }
        stage('Integration Test') {
            steps {
                sh "mvn verify -DskipTests=True"
            }
        }
        stage('Package') {
            steps {
                sh "mvn package -DskipTests=True"
            }
        }
    }
}