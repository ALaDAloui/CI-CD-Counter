pipeline{

    agent any
     tools {
          maven 'maven'

        }

    stages {

         stage('Git Checkout'){

            steps{

                script{

                    git branch: 'main', url: 'https://github.com/ALaDAloui/CI-CD-Counter.git'
                }
            }
       }

       stage('UNIT testing'){

                   steps{

                       script{

                           sh 'mvn test'
                       }
                   }
              }
       stage('Integration testing'){

                          steps{

                              script{

                                  sh 'mvn verify -DskipUnitTests'
                              }
                          }
                     }
   }
}