pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    triggers {
        githubPush()
    }

    parameters {
        string(name: 'APP_NAME', defaultValue: 'my-app', description: 'Application Name')
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Select Environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests or not')
    }

    environment {
        VERSION = "1.0.${env.BUILD_NUMBER}"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/BhanuSaiReddy/jenkins.git'
            }
        }

        stage('Webhook Check') {
            steps {
                script {
                    def cause = currentBuild.rawBuild.getCause(hudson.triggers.SCMTrigger$SCMTriggerCause)

                    if (cause) {
                        echo "Triggered by Webhook"
                    } else {
                        echo "Triggered manually"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building ${params.APP_NAME} version ${VERSION}"
                sh 'echo Build step...'
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                echo "Running tests for ${params.APP_NAME}"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying ${params.APP_NAME} to ${params.ENV}"

                    if (params.ENV == 'prod') {
                        input message: "Approve PROD deployment?", ok: "Deploy"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "SUCCESS: ${params.APP_NAME} deployed to ${params.ENV}"
        }
        failure {
            echo "FAILURE: ${params.APP_NAME}"
        }
        always {
            cleanWs()
        }
    }
}