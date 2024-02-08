pipeline{
    agent{
        node{
            label 'jenkins-minion'
        }
    }
    stages{
        stage('SonarQube Analysis'){
            steps{
                script{
                    def scannerHome = tool 'sonarscanner';
                    withSonarQubeEnv() {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Quality Gateway'){
            steps{
                timeout(time: 1, unit: 'HOURS'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Build'){
            steps{
                script{
                    sh '''
                        sudo systemctl stop chatapp.service
                        rsync -avz $WORKSPACE jenkins@172.17.9.172:/tmp/
                        ssh jenkine@172.17.9.172 "sudo -u chatapp /usr/local/bin/cicd.sh"
                        sudo systemctl start chatapp.service
                    '''
                }
            }
        }
    }
}
