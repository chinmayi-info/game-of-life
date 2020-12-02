node('HRMS&&QA') {
    try {
        def responses = null
        stage('selection'){
            responses = input message: 'Enter branch and select build type',
            parameters: [string(defaultValue: '', description: 'Enter the branch', name: 'BRANCH_NAME'),
                        choice(choices: 'DEBUG\nRELEASE', description: 'BUILD Type', name: 'BUILD_TYPE')]

        }

        stage('git') {
            git 'https://github.com/chinmayi-info/game-of-life.git'
        }
        stage('build') {
            if (responses.BUILD_TYPE == 'DEBUG'){
                sh 'mvn clean package'
            }
            if (responses.BUILD_TYPE == 'RELEASE'){
                sh 'mvn clean install'
            }
            
        }
        stage('testresults'){
            junit 'gameoflife-web/target/surefire-reports/*.xml'
        }
        stage('archiveartifacts') {
            archiveArtifacts artifacts: 'gameoflife-web/target/*.war', followSymlinks: false
        }
        currentBuild.result = 'SUCCESS'

    }
    catch(err) {
        currentBuild.result = 'FAILURE'
    }
    finally {
        mail to: 'rajashekar.devops31@gmail.com', 
             subject: "Status of the build: ${currentBuild.fullDisplayName}",
             body: "${env.BUILD_URL} has ${currentBuild.result}"
    }
    
}