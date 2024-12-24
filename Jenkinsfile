pipeline {
    agent {
        node {
            label any
            customWorkspace '/some/other/path'
        }
    }
    
    stages {
        stage('Clean Workspace') { 
            steps {
                cleanWs()
                script {
                        echo "env.BRANCH_NAME : ${env.BRANCH_NAME} and env.JOB_NAME : ${env.JOB_NAME} and workspace is ${WORKSPACE}"
                    }
            }
        }
        stage('Git Checkout') { 
            steps {

                dir("$WORKSPACE"){
                    checkout scm: scmGit(
                        branches: [[name: "$BRANCH_NAME"]],
                        extensions: [], 
                        userRemoteConfigs: [
                            [url: 'https://github.com/Maspapa/TestCode.git']
                        ]
                    )
                }
            }
        }
        stage('Copy Dependences') {
            steps {
                copyArtifacts filter: '*.md', fingerprintArtifacts: true, projectName: 'Mason/1_build/$BRANCH_NAME', selector: lastSuccessful(), target: './'

            }
        }
    }
    post {
        success {
            archiveArtifacts artifacts: '**/*', fingerprint: true, onlyIfSuccessful: false, defaultExcludes: false
        }
    }
}
