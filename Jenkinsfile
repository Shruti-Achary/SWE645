pipeline {
   environment {
        registry="sachar6/studentsurvey645"
        registryCredential='Docker'
        TIMESTAMP=new Date().format("yyyyMMdd_HHmmss")
    }
   agent any

   stages {
      stage('Build') {
         steps {
            script{
               sh 'rm -rf *.war'
               sh 'jar -cvf studentsurvey.war -C main/webapp .'
               //sh 'echo ${BUILD_TIMESTAMP}'

               docker.withRegistry('https://index.docker.io/v1/', registryCredential){
                  def customImage=docker.build("sachar6/studentsurvey645:${env.TIMESTAMP}")
               }
            }
         }
      }

      stage('Push Image to Dockerhub') {
         steps {
            script{
               docker.withRegistry('https://index.docker.io/v1/', registryCredential){
                  sh "docker push sachar6/studentsurvey645:${env.TIMESTAMP}"
               }
            }
         }
      }

      stage('Deploying Rancher to single pod') {
         steps {
            script{
               sh "kubectl set image deployment/one-life-deployment container-0=sachar6/studentsurvey645:${env.TIMESTAMP}"
            }
         }
      }
      
      stage('Deploying to Rancher using Load Balancer as a service') {
         steps {
            script{
               sh "kubectl set image deployment/one-life-deployment container-0=sachar6/studentsurvey645:${env.TIMESTAMP}"
            }
         }
      }
   }
}
