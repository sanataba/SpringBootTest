pipeline {
    agent {
        label 'new123'
    }
    environment {
        registry = "706274417810.dkr.ecr.ap-south-1.amazonaws.com/springboot"
    }
       stages{
          stage('git'){
           steps{
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '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', url: 'https://github.com/sanataba/SpringBootTest']]])
           }
        } 
          stage('Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        } 
        stage('docker build'){
            steps{
                script{
                    sh 'docker build -t springboot .'
                    sh 'docker tag springboot:latest 706274417810.dkr.ecr.ap-south-1.amazonaws.com/springboot:latest'
                }
            }
        }
  /*     stage('push docker image'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u sanataba -p ${dockerhubpwd}'
                    sh 'docker push sanataba/springboot'
}
                    
                }
            }
        }   */
         stage('aws login'){
            steps{
              sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 706274417810.dkr.ecr.ap-south-1.amazonaws.com'
          }
      } 
         stage('DockerPush to AWS ECR'){
            steps{
                sh 'docker push 706274417810.dkr.ecr.ap-south-1.amazonaws.com/springboot:latest'
            }
        } 
         stage ('eks Deploy') {
           steps {
               script{
                    withKubeConfig([credentialsId: 'K8s']) 
                    {
        
              sh 'kubectl apply -f Deployment.yaml'
        }
        }
    
      }
}
                  
                  
    }
}

