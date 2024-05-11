pipeline {
  agent any
    tools{
      maven 'MAVEN_HOME'
          }
 
   stages {
    stage('Git checkout') {
      steps {
         echo 'This is for cloning the gitrepo'
         git branch: 'main', url: 'https://github.com/niranjan-1001/Banking-Demo666.git'
                          }
            }
    stage('Create a Package') {
      steps {
         echo 'This will create a package using maven'
         sh 'mvn clean package'
                             }
            }

    stage('Publish the HTML Reports') {
      steps {
          publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Banking/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
            } 
    stage('Create a Docker image') {
      steps {
        sh 'docker build -t niranjan1001/banking:1.0 .'
                    }
            }
    stage('Login to Dockerhub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'Dockerlogin-user', passwordVariable: 'dockerhubpass', usernameVariable: 'dockerhublogin')]) {        
        sh 'docker login -u ${dockerhublogin} -p ${dockerhubpass}'
                                                                    }
                                }
            }
    stage('Push the Docker image') {
      steps {
        sh 'docker push niranjan1001/banking:1.0'
                                }
            }
    stage('Create Infrastructure using terraform') {
      steps {
            dir('scripts') {
            withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkinsIAMuser', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            sh 'terraform init'
            sh 'terraform validate'
            sh 'terraform apply --auto-approve -lock=false'
                      }
                 }
            }
        }

    }
}
