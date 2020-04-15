pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Build image') { 
            steps {
                script {
                    echo "Build image with tag: ${env.BUILD_ID}"
                    dockerImage = docker.build("172.28.128.3:30700/nodejs-k8s-cicd:${env.BUILD_ID}")
                }
            }
        }
        stage('Push image to registry') { 
            steps {
                script {
                    docker.withRegistry('http://172.28.128.3:30700', 'nexus-docker') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy to K8s cluster') { 
            steps {
                script {
                    withCredentials([kubeconfigFile(credentialsId: 'k8s-config', variable: 'KUBECONFIG')]) {
                        sh '''
                            kubectl version
                            sed -i 's/latest/'"${BUILD_ID}"'/g' nodejs-k8s-cicd-k82-deployment.yaml
                            kubectl --kubeconfig=${KUBECONFIG} apply -f nodejs-k8s-cicd-k82-deployment.yaml
                        '''
                    }
                    
                }
            }
        }
        stage('Remove unused docker image') { 
            steps {
                sh "docker image rm 172.28.128.3:30700/nodejs-k8s-cicd:${env.BUILD_ID}"
            }
        }
    }
}