pipeline {
    agent any

    options {
        timestamps() // Add timestamps to logging

        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
    }

    parameters {
         string(name: 'testcase_id', defaultValue: 'f225eeca-2856-4210-a50f-dbf6b0a34b71', description: 'Test Manager testcase_id')
    }
    environment {
        PATH = "/usr/local/bin:$PATH"
    }

    stages {
        stage('Init') {
            steps {
                echo 'Stage: Init'
                echo "branch=${env.BRANCH_NAME}, testcase_id=${params.testcase_id}"
            }
        }
        stage('Common Config') {
            steps {
                echo 'Stage: Common Config'
                // Checkout repo with common config files/scripts to 'common' folder
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          doGenerateSubmoduleConfigurations: false, 
                          extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'common']], 
                          submoduleCfg: [], userRemoteConfigs: [[url: 'file:///home/jenkins/gitrepos/cicd-common']]
                         ])
            }
        }
        stage('Test Manager') {
            steps {
                echo 'Stage: Test Manager'
                dir('common') {
                    sh """
                    ./tm_start_tc.sh "${params.testcase_id}"
                    """
                }
                echo 'Stage: Test Manager complete'
            }
        }
        stage('Cleanup') {
            steps {
                echo 'Stage: Cleanup'
                // deleteDir()
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}

