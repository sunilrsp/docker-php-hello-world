  pipeline {
    agent any
       
    stages {
    
  
    stage('Build Docker image'){
                       
            steps {
                script {
                    app = docker.build("world2enjoy/phoenix2.0")
                    app.inside {
                        sh 'echo $(curl localhost:80)'
                    }
                }
            }
            
        }
        
        stage('Push docker image'){
           
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com','docker_hub_login'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        
        stage('Deploy kubernetes'){
          
          steps {
            sh 'kubectl delete -f application.yaml'
             kubernetesDeploy(
                kubeconfigId: 'kubeconfig',
                configs: 'application.yaml',
                enableConfigSubstitution: true)
                echo 'App url: http://54.186.233.130:30005'
          }
                     
        }
        
    }
}
