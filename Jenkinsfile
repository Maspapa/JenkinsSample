pipeline {
    agent {
        node {
            label 'main'
            customWorkspace "${env.JENKINS_HOME}/workspace/${env.JOB_NAME}"
        }
    }
    stages {
        stage('Clean Workspace') { 
            steps {
                cleanWs()
                script {
                        echo "env.BRANCH_NAME : ${env.BRANCH_NAME} and env.JOB_NAME : ${env.JOB_NAME.replaceAll('/', '\\')} and workspace is ${WORKSPACE} and JENKINS_HOME is  ${env.JENKINS_HOME}"
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
