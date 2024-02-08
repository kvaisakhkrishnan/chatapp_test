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
                        ssh jenkins@172.17.9.142 "sudo systemctl stop chatapp.service"
                        rsync -avz $WORKSPACE jenkins@172.17.9.172:/tmp/
                        ssh jenkins@172.17.9.142 "sudo -u chatapp /usr/local/bin/cicd.sh"
                        ssh jenkins@172.17.9.142 "sudo systemctl start chatapp.service"
                    '''
                }
            }
        }
    }
}
