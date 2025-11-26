pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
    }

    environment{
        DEBUG = 'true'
        appVersion = '' // this will become global variable and can be used across pipeline
    }

    stages {
        stage('Read the version') {
            steps {
                script{
                    def packageJson = readJson file: 'package.json'
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}"
                }
            }
        }

        stage('Test') {
            steps {
                echo "This is test"
            }
        }

        stage('Deploy') {
            when{
                branch 'main'
            }
            steps {
                echo "This is deploy"
            }
        }

        // asks for approval
        stage('Approval') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice, bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }

            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }

    post {
        always {
            echo "This section run irrespective of success or failure"
            deleteDir() // this will create the workspace files in jenkins agent
        }

        success {
            echo "This section runs when pipeline is success"
        }

        failure {
            echo "This section runs when pipeline fails"
            // we can integrate slack or any other message service, so when the Build fails it notifies in slack. 
        }
    }
}
