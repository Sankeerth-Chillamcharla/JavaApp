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
         stage('Sonar Analysis'){
    steps{
        withSonarQubeEnv('sonarqube') {
         sh "mvn clean verify sonar:sonar -Dsonar.projectKey=javaapp -Dsonar.projectName='javaapp'"
}
    }
 }
 stage("Quality Gate"){
    steps {

    
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }} }
        stage('Code Cleaning') {
            steps {
                sh "mvn clean"
            }
        }
        stage('Integration Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Package') {
            steps {
                sh "mvn package -DskipTests=True"
            }
        }
    }
}
