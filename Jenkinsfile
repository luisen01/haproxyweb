pipeline{
  agent {
    node{    
      label 'VPS'
      }
  }
    environment {
        dockerfilePath= "/opt/haproxyweb/"
        registry = "luisen01/haproxyweb"
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
                sh 'docker stop apache2'
                sh 'docker rm apache2'
                sh 'docker run -d -p 9090:80 --name apache2 ${registry}'
            }
        }
    }
}
