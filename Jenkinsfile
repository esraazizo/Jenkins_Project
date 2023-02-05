pipeline {
    agent { label 'slave-one' }

    stages {
        stage('Build And Push') {
            steps {
                echo 'Building..'
                sh 'ls'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
