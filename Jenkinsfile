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
       stage('Maven Build'){

                                 steps{

                                     script{

                                         sh 'mvn clean install'
                                     }
                                 }
                            }
       stage('Static Code Analysis'){

                   steps{

                       script{

                           withSonarQubeEnv(credentialsId: 'sonar-api-1') {
                               sh 'mvn clean package sonar:sonar'
                           }
                       }
                   }
              }
        stage('Quality Gate Status'){

                                         steps{

                                             script{

                                                 waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-1'
                                             }
                                         }
                                    }
   }

}