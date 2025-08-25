pipeline {
    agent any
    parameters {
        booleanParam(name: 'RUN_DEPLOY', defaultValue: true, description: 'Should we deploy?')
         choice(
            name: 'DEPLOY_ENV',
            choices: ['dev', 'staging', 'prod'],
            description: 'Selecting environment for deploy'
        )
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }
        stage('Test in Parallel') {
            parallel {
                stage('Unit Tests') {
                    when {
                        expression { currentBuild.currentResult == 'SUCCESS' }
                    }
                    steps {
                        echo 'Running unit tests...'
                        sh 'sleep 5'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        echo 'Running integration tests...'
                        sh 'sleep 5'
                    }
                }
            }
        }

        stage('simulate testing') {
            parallel {
                stage('Window') {
                    steps {
                        echo 'Running on Window'
                    }
                }
                stage('Linux') {
                    steps {
                        echo 'Running on Linux'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                sh 'echo "All tests passed!" > results.txt'
                archiveArtifacts artifacts: 'results.txt', fingerprint: true
            }
        }

        stage('List File') {
            steps {
                sh 'ls -l'
                sh 'exit 1'
            }
        }
         stage('Approval') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    input "Do you want to proceed with deployment?"
                }  
            }
        }
        

        stage('Deploy') {
            when {
                expression { return params.RUN_DEPLOY }
            }
            steps {
                echo "Deploying the application to ${params.DEPLOY_ENV} environment..."
            }

        } 
        
    }

    post {
        success {
            echo '✅ Pipeline finished successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs!'
        }
        always {
            echo 'Pipeline completed (success or failure).'
        }
    }

}

    
