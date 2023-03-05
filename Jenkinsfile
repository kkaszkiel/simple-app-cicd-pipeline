pipeline {
   agent any
   environment {
       GOCACHE = "/tmp"
       registry = "kkaszkiel/simple-app"
   }
   stages {
       stage('Build') {
           agent {
               docker {
                   image 'golang:1.20.1'
               }
           }
           steps {
               // Create project directory.
               sh 'cd ${GOPATH}/src'
               sh 'mkdir -p ${GOPATH}/src/simple-app'
               // Copy all *.go files to project directory.               
               sh 'cp -r ${WORKSPACE}/*.go ${GOPATH}/src/simple-app'
               // Build the app.
               sh 'go build -o main main.go'              
           }    
       }
       stage('Test') {
           agent {
               docker {
                   image 'golang:1.20.1'
               }
           }
           steps {                

               sh 'cd ${GOPATH}/src'
               sh 'mkdir -p ${GOPATH}/src/simple-app'       
               sh 'cp -r ${WORKSPACE}/*.go ${GOPATH}/src/simple-app'
               // Run tests.
              sh 'go test main_test.go '
                        
           }
       }
       stage('Publish') {
           environment {
               registryCredential = 'dockerhub'
           }
           steps{
               script {
                   def appimage = docker.build registry + ":$BUILD_NUMBER"
                   docker.withRegistry( '', registryCredential ) {
                       appimage.push()
                       appimage.push('latest')
                   }
               }
           }
       }
       stage ('Deploy') {
           steps {
               script{
                   def image_id = registry + ":$BUILD_NUMBER"
                   sh "ansible-playbook  playbook.yml --extra-vars \"image_id=${image_id}\""
               }
           }
       }
   }
}
