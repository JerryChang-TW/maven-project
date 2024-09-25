pipeline{
    agent any
    parameters{
        string(name:'DEV_STAGING', defaultValue: '35.77.213.93', description: 'deploy to staging')
        string(name:'DEV_PRODUCTION', defaultValue: '18.183.252.55', description: 'deploy to production')
    }
    environment{
        MAVEN_HOME='/opt/apache-maven-3.9.9'
    }
    triggers{
        pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps{
               sh '${MAVEN_HOME}/bin/mvn clean package'
            }
            post{
                success{
                    echo 'starting save file'
                    archiveArtifacts(artifacts: '**/*.war')
                }
            }
        }
        stage('Deploy'){
            parallel{
                stage('staging'){
                    steps{
                       sh "scp **/*.war ubuntu@${params.DEV_STAGING}:/var/lib/tomcat10/webapps"
                    }
                    post{
                        success{
                            echo "deploy to staging clear"
                        }
                        failure{
                            echo "deploy to stagin fail"
                        }
                    }
                }
                stage('production'){
                    steps{
                        sh "scp **/*.war ubuntu@${params.DEV_PRODUCTION}:/var/lib/tomcat10/webapps"
                    }
                    post{
                        success{
                            echo "deploy to staging clear"
                        }
                        failure{
                            echo "deploy to stagin fail"
                        }
                    }
                }
            }
        }
    }
}
