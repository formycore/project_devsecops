pipeline{
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    stages {
        stage('Clean workspace'){
            steps {
                cleanWs()
            }
        }
    }
}