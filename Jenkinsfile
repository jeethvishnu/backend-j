pipeline {
    agent {
        label 'agent-2'
    }

    options {
        timeout(time: 15, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }

    environment {
        // appVersion is declared here for global use, but it's better initialized inside 'script' for scoping
        def appVersion = ''
    }

    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "appVersion: ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    cd backend
                    npm install
                    ls -ltr
                    echo "appVersion: ${appVersion}"
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                    ls -ltr
                '''
            }
        }
    }

    post {
        always {
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success {
            echo 'Will run if it is successful'
        }
        failure {
            echo 'Will run when it has failed'
        }
    }
}