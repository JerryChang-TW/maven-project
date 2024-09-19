pipeline{
    agent any 
    tools{
        maven 'local maven'
    }
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success {
                    echo '開始存檔...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('deploy to staging'){
            steps{
                build job: 'deploy-to-staging'
            }
        }
        stage('deploy to production'){
            steps{
                timeout(time: 5, unit: 'DAYS'){
                    input(message: '是否部署到生產環境')
                }

                build(job: 'deploy-to-production')
            }
            post{
                success{
                    echo(message: '代碼成功部署到生產環境')
                }
                failure{
                    echo(message: '部署失敗')
                }
            }
        }
    }
}
