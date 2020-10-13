pipeline {
    agent { docker 'php' }
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
