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

       stage('Upload war file to nexus'){

                                steps{

                                     nexusArtifactUploader artifacts:
                                      [[artifactId: 'springboot',
                                       classifier: '',
                                       file: 'target/Uber.jar',
                                        type: 'jar']
                                        ],
                                         credentialsId: 'nexus-auth',
                                          groupId: 'com.exemple',
                                           nexusUrl: 'localhost:8081',
                                            nexusVersion: 'nexus3',
                                             protocol: 'http',
                                             repository: 'counterapp-release',
                                             version: '1.0.0'
                                              }
                                         }
                                 }

   }

}