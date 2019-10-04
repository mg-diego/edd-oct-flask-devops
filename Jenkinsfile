void setBuildStatus(String message, String state) {
    step([
        $class: "GitHubCommitStatusSetter",
        contextSource: [
            $class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"
        ],
        errorHandlers: [
            [$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]
        ],
        statusResultSource: [ $class: "ConditionalStatusResultSource",
            results: [[$class: "AnyBuildResult", message: message, state: state]]
        ]
    ]);
}

pipeline {
    agent any
    environment {
        DEPLOY_URL = ''
        GROUP_NAME = 'Group2'
        GROUP_PORT = '5002'
        PROJECT_NAME = 'edd-oct-flask-devops'
        PACKAGE_NAME = 'apis'
        LOCAL_BRANCH_NAME = ''
        CURRENT_GIT_COMMIT = ''
        CONTAINER_NAME = ''
        CURRENT_IMAGE_NAME = ''
        PREVIOUS_IMAGE_NAME = ''
    }
    stages {
        stage('Info') {
            steps {
                echo 'Starting'
                script {
                    def scmVars = checkout scm
                    LOCAL_BRANCH_NAME = scmVars.GIT_BRANCH
                    CURRENT_GIT_COMMIT = scmVars.GIT_COMMIT
                    DEPLOY_URL = BUILD_URL.split('/')[2].split(':')[0]
                    echo DEPLOY_URL
                    echo "Branch Name : " + LOCAL_BRANCH_NAME
                    echo "Commit SHA  : " + CURRENT_GIT_COMMIT
                    CONTAINER_NAME = "testing-flask-$GROUP_NAME"
                    TEMPLATE_IMAGE_NAME = "testing-flask-image-$GROUP_NAME"
                    CURRENT_IMAGE_NAME = "$TEMPLATE_IMAGE_NAME-$CURRENT_GIT_COMMIT"
                    PREVIOUS_IMAGE_NAME = sh (
                        script: "docker ps -f name=$CONTAINER_NAME -q | xargs --no-run-if-empty docker inspect --format='{{.Config.Image}}' $CONTAINER_NAME",
                        returnStdout: true
                    ).trim()
                    echo "Container Name : " + CONTAINER_NAME
                    echo "Current Image Name : " + CURRENT_IMAGE_NAME
                    echo "previous Image Name : $PREVIOUS_IMAGE_NAME"
                }
            }
        }

        stage('Linter') {
            steps {
                echo 'Linting...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
        stage('Health-check') {
            steps {
                echo "Health-check"
            }
        }
    }
    post {
        failure {
            script {
                sh "echo POST-ACTION failure"
            }
        }
        success {
            script {
                sh "echo POST-ACTION success"
            }
        }
        always {
            sh "echo POST-ACTION always"
        }
    }
}