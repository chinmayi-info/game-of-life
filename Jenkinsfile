pipeline {
    agent { label 'HRMS&&QA' }
    triggers { cron('30 * * * 1-5') }
    parameters {
        string(
            name: 'TestResultsPath', 
            defaultValue: 'gameoflife-web/target/surefire-reports/*.xml', 
            description: 'Test Results Path'
        )
        string(
            name: 'ArtifactsToBeArchive',
            defaultValue: 'gameoflife-web/target/*.war',
            description: 'This is archival path'

        )

        choice(
            name: 'BUILD_TYPE',
            choices: ['Debug', 'Release'],
            description: 'Build Type'

        )

        choice(
            name: 'BranchToBuild',
            choices: ['declarativepipeline', 'scriptedpipeline', 'master'],
            description: 'Branch to be build'
        )
    }
    stages {
        stage('git'){
            steps {
                git branch: "${params.BranchToBuild}", url: 'https://github.com/chinmayi-info/game-of-life.git'
            }
        }
        stage('debug build') {
            when {
                allOf {
                    expression { params.BUILD_TYPE == 'Debug'}
                }
            }
            steps {
                sh 'mvn clean package'
            }
        }
        stage('release build') {
            when {
                allOf {
                    expression { params.BUILD_TYPE == 'Release'}
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }
        stage('postbuild') {
            steps {
                // junit test Results
                junit testResults: "${params.TestResultsPath}"
                // archive the artifacts
                archive includes: "${params.ArtifactsToBeArchive}"
            }
        }
    }
}
