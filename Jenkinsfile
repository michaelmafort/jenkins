pipeline {
    agent { docker image: 'php:alpine' }
    environment {
        DB_USER = 'root'
    }
    stages {
        stage('build') {
            steps {
                sh 'php --version'
                sh 'echo $DB_USER > DB.env'
                sh "php -r 'print(\"Hello world!\");'"
                sh "php -r 'file_put_contents(\"VERSION\", \"2.1.0\");'"
            }
        }
        stage('Everything is ok') {
            steps {
                input "Does the build look ok?"
            }
        }
        stage('test') {
            steps {
                sh '''
                    cat DB.env
                    php -r "exit(file_get_contents('VERSION') == '2.1.0' ? 0 : 1);"
                '''
            }
        }
        stage('deploy') {
            steps {
                sh 'echo "Deploying..."'
            }
        }
    }
    post {
        always {
            echo 'Finished!'
        }
        failure {
            echo "Fail: build stoped."
        }
    }
}