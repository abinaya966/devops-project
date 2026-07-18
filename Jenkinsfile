pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                credentialsId: 'github',
                url: 'https://github.com/abinaya966/devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t abinaya1901/nginx-app:${BUILD_NUMBER} app/'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin
                    docker push abinaya1901/nginx-app:${BUILD_NUMBER}'
                    '''
             }
          }
        }
        stage('Update Helm Values') {
             steps {
                    sh """
                    sed -i 's/tag:.*/tag: ${BUILD_NUMBER}/' helm/values.yaml
                    cat helm/values.yaml
                    """
    }
}
                }
            }
        }
    }
}
