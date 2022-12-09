pipeline{
    agent any
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/gundepallieswartej/jenkins-docker-example.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-8.9.9') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn sonar:sonar"
    }
        }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t gundepallieswartej/devops-automation .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhubcred', variable: 'dockerhubcred')]) {
                   sh 'docker login -u eswargundepalli -p ${dockerhubcred}'

}
                   sh 'docker push gundepallieswartej/devops-automation'
                }
            }
        }
        
    }
}
