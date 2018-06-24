pipeline {
    agent {
        docker { image 'ehemmerlin/payara-full' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'docker-compose –f docker-compose-prod.yml'
            }
        }
    }
}
