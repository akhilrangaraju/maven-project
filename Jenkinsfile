pipeline {
    agent any
    
    triggers {
         pollSCM('* * * * *')
     }

    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        // stage('Deploy to Staging'){
        //     steps{
        //     build job: 'deploy-to-staging'
        //     }
        // }

        // stage('Deploy to Production'){
        //     steps{
        //         timeout(time:5, unit:'DAYS'){
        //             input message: 'Approve PRODUCTION deployment?'
        //         }
        //             build job: 'deploy-to-prod'
        //     }
        //     post{
        //         success {
        //             echo 'Deployment to Production Successful' 
        //         }

        //         failure {
        //             echo 'Deployment to Production Failed'
        //         }
        //     }
            
        // }

        stage('Deployments'){
            parallel{
                stage('Deploy to QA'){
                    steps{
                        sh "docker cp **/target/*.war cc66f428534e:/webapps"
                    }
                }

                stage('Deploy to PROD'){
                    steps{
                        timeout(time:5, unit:'DAYS'){
                            input message: 'Approve PROD deployment?'
                        }

                        sh "docker cp **/target/*.war 97a9af5ffaf0:/webapps"
                    }
                    post{
                        success {
                            echo 'Deployment to Production Successful'
                        }
                        failure {
                            echo 'Deployment to Production Failed'
                        }
                    }
                    
                }
            }
        }

    }
    
}