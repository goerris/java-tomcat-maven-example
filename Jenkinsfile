pipeline {
    agent any
        stages {
            stage ('Build Servlet Project') {
                steps {
                  bat 'mvn clean package'
                }
        

            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts : '**/*.war'
                }
            }
        }
            stage ('Deploy Build in Staging Area') {
                steps {
                    build job : 'Deploy-StagingArea-Pipeline'
            }
        }

            stage ('Deploy to Production'){
                steps {
                    timout (time: 5, unit: 'Days'){
                        input message: 'Approve Production Deployment?'
                    }

                    build job : 'Deploy-Production-Pipeline'
                }
                post{
                    success{
                        echo 'Deployment on Production Successful'
                    }

                    failure{
                        echo 'Deployment on Production Failed'
                    }
                }
            }
    }
}
