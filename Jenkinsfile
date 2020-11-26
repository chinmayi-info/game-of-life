pipeline {
    agent { label 'HRMS&&QA' }
    stages {
        stage('git'){
            steps {
            git 'https://github.com/chinmayi-info/game-of-life.git'
     }
        }
        stage('compile') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}