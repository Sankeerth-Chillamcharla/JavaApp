pipeline {
    agent any
    tools {
        maven "Maven 3.9.2"
    }
    stages {
        stage('Pull Code from Git Hub') {
            steps {
                // Pull the code form SCM Repo ---> git Hub
                git 'https://github.com/Sankeerth-Chillamcharla/JavaApp.git'
            }
        }
        stage('Clean') {
            steps {
                sh "mvn clean"
            }
        }
        stage('Package') {
            steps {
                sh "mvn package -DskipTests=True"
            }
        }
    }
}
