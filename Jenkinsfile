pipeline {
    agent { label 'slave-one' }
   
    stages {
        stage('build') {
            steps {
                script {
                   if (BRANCH_NAME == "release") {
                       withCredentials([usernamePassword(credentialsId: 'esraa-docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                           sh """
                                docker login -u $USERNAME -p $PASSWORD
                                docker build -t esraazizo/esraa-bakehouse:${BUILD_NUMBER} .
                                docker push esraazizo/esraa-bakehouse:${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../esraa-sbakehouse.txt
                           """
                       }
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    if (BRANCH_NAME == "dev" || BRANCH_NAME == "test" || BRANCH_NAME == "prod") {
                            withCredentials([file(credentialsId: 'k8s-conf', variable: 'KUBECONFIG')]) {
                          sh """
                              export BUILD_NUMBER=\$(cat ../esraa-bakehouse.txt)
                              mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                              cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                              rm -f Deployment/deploy.yaml.tmp
                              kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
                            """
                        }
                    }
                }
            }
        }
    }
}
