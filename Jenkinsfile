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
    }
}
