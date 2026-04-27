pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    // 🔹 Global variables
    environment {
        APP_NAME = "my-app"
        BUILD_ENV = "dev"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Build') {
            steps {
                script {
                    // 🔹 Scripted block
                    def buildMsg = "Building ${APP_NAME} in ${BUILD_ENV} environment"
                    echo buildMsg

                    // Example shell
                    sh 'echo Build step running...'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // 🔹 Conditional logic (scripted)
                    def runTests = true

                    if (runTests) {
                        echo "Running tests..."
                        sh 'echo Test execution...'
                    } else {
                        echo "Skipping tests..."
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying ${APP_NAME}..."

                    // 🔹 Try-Catch (scripted feature)
                    try {
                        sh 'echo Deployment started...'
                    } catch (Exception e) {
                        echo "Deployment failed: ${e}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build Successful for ${APP_NAME}"
        }

        failure {
            echo "❌ Build Failed for ${APP_NAME}"
        }

        always {
            cleanWs()
        }
    }
}