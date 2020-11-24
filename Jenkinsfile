node('HRMS && QA') {
    properties([pipelineTriggers([cron('30 * * * 1-5')])])
    stage('git') {
        git 'https://github.com/chinmayi-info/game-of-life.git'
    }
    stage('build') {
        sh 'mvn clean package'
    }
    stage('testresults'){
        junit 'gameoflife-web/target/surefire-reports/*.xml'
    }
    stage('archiveartifacts') {
        archiveArtifacts artifacts: 'gameoflife-web/target/*.war', followSymlinks: false
    }
}