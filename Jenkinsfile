pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script  {
                    dockerapp = docker.build("olatejulian/kube-news:${env.BUILD_ID}", '-f ./Dockerfile ./')
                }
            }
        }

        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy to Kubernetes') {
            steps {
                withKubeconfig ([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
                }
            }
        }
    }
}