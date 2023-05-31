pipeline{
    agent any 
    tools { 
        maven "jenkinsmaven"
        jdk "jenkisjava"
        }
      stages{
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/renejota/maveninventario.git'
            }
          }
        
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }   
         stage('archiveArtifacts') {
             steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
          stage('Test') {
              steps {
                script {
                    sh 'mvn test'
                }
            }
            
        }
        stage('Sonar Scanner') {
            steps {
              script {
                def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                    sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://SonarQube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=mv-maven -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/java/com/kibernumacademy/miapp/App.java -Dsonar.tests=srcsrc/test/java/com/kibernumacademy/miapp/AppTest.java -Dsonar.language=java -Dsonar.java.binaries=."
                }
            }
        }
    }
        
     }
     
     post {
        always {
            cleanWs()
        } 
         
     }
}     