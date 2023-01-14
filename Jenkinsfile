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

                                     script {
                                     def readPomVersion = readMavenPom file: 'pom.xml'
                                     def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT")?"counterapp-snapshot":"counterapp-release"

                                     nexusArtifactUploader artifacts:
                                      [[artifactId: 'springboot',
                                       classifier: '',
                                       file: 'target/Uber.jar',
                                        type: 'jar']
                                        ],
                                         credentialsId: 'nexus-auth',
                                          groupId: 'com.example',
                                           nexusUrl: '172.22.0.4:8081',
                                            nexusVersion: 'nexus3',
                                             protocol: 'http',
                                             repository: nexusRepo,
                                             version: "$readPomVersion.version"
                                              }
                                         }
                                 }

   }

}