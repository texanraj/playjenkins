pipeline {

  environment {
    dockerImage = "texanraj/welcomeapp"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/texanraj/playjenkins.git'
      }
    }

        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(dockerImage)
                    app.inside {
                        sh 'echo Lets go ASTROS !!!'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}
