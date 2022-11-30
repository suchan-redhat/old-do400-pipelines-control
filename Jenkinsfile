pipeline {
    agent {
        node {
            label 'nodejs'
        } 
    }
    parameters {
        booleanParam(name: "RUN_FRONTEND_TESTS", defaultValue: true)
    }
    stages{
        stage('checkout') {
            steps {
                git branch: 'test', url: 'https://github.com/suchan-redhat/DO400-apps-20221128.git' 
            }
        }
        stage('Run Tests') {
            parallel {
                stage('Backend Tests') {
                    steps {
                        sh 'node ./backend/test.js'
                    } 
                }
                stage('Frontend Tests') {
                    when { expression { params.RUN_FRONTEND_TESTS } }
                    steps {
                        sh 'node ./frontend/test.js'
                    }
                } 
            }
        }
        stage('Deploy') {
            when {
                expression { env.GIT_BRANCH == 'origin/main' }
                beforeInput true
            }
            input {
                message 'Deploy the application?'
            }
            steps {
                echo 'Deploying...'
            } 
        }
    }
}
