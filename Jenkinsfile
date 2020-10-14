node {
    checkout scm
    def customImage = docker.build("my-image:latest")
    customImage.inside {
        sh 'subversion --version'
        sh 'docker images'
    }
}
pipeline {
    agent { 
        docker {
            image "php"
            args "-v .:/var/www/symfony"
        } 
    }
    stages {
        stage('build') {
            steps {
                sh 'php --version'
                sh 'echo "hello world"'
                sh '''
                    echo "Multiline shell steps works too"
                                        ls -lah
                '''
            }
        }
    }
    post {
        always {
            echo "bhild finished"
        }
        success {
            echo "success"
        }
        failure {
            echo "failure"
        }
        unstable {
            echo "unstable"
        }
        changed {
            echo "changed"
        }
    }
}
