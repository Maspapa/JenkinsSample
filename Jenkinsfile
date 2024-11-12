pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        // stage('Git Checkout') { 
        //     steps {

        //         dir("$WORKSPACE"){
        //             checkout scm: scmGit(
        //                 branches: [[name: "$BRANCH_NAME"]],
        //                 extensions: [], 
        //                 userRemoteConfigs: [
        //                     [url: 'https://github.com/Maspapa/TestCode.git']
        //                 ]
        //             )
        //         }
        //     }
        // }

        stage('Copy Dependences') {
            steps {
                copyArtifacts(
                    projectName: "Mason/1.build/${BRANCH_NAME}",
                    selector: specific('lastSuccessful'),
                    filter: '*.md',
                    fingerprintArtifacts: true,
                    target: './'
                )
            }
        }
    }
    post {
        success { 
            archiveArtifacts artifacts: '**/*', fingerprint: true, onlyIfSuccessful: false, defaultExcludes: false
        }
    }
}