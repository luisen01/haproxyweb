pipeline {
    agent any

    environment {
        dockerfilePath= "/opt/haproxyweb/"
        registry = "luisen01/jenkins"
        registryCredential = 'dockerhub_id'
    }
    stages {
        stage ('Test') {
            steps {
                sh 'echo executing test'
            }
        }
        stage ('Build Docker') {
            steps {
                script {
                    dockerImage = docker.build(registry, "-f ${dockerfilePath}Dockerfile .")
                }
            }
        }
        stage ('Push Docker') {
            steps {
                script {
                    docker.withRegistry('', registryCredential){
                        dockerImage.push()
                    }
                }
            }
        }
        stage ('Run Docker'){
            steps{
                sh 'docker run -d -p 9091:80 --name apache2 ${registry}'
            }
        }
    }
}
