pipeline{
    agent any 
    tools { 
        maven "jenkinsmaven"
        }
      stages{
        stage('checkout') {
            steps {
               checkout([
                   $class: 'GitSCM', 
                   branches: [[name: '*/main']], 
                   doGenerateSubmoduleConfigurations: false, 
                   extensions: [[$class: 'CleanCheckout']], 
                   submoduleCfg: [], 
                   userRemoteConfigs: [[credentialsId: 'sshconexiongithub', url: 'git@github.com:renejota/maveninventario.git']]
            ])
                  }
           }
        stage('Build') {
            steps {
                sh 'mvn -version'
                sh 'mvn clean install'
                sh 'mvn -B -DskipTests clean package'
            }
        }   
         stage('archiveArtifacts') {
            steps {
                sh 'mkdir jar'
                sh 'echo "not a artifact file" > jar/build.jar'
                sh 'echo "artifact file" > jar/build.min.jar'
            }
        }
          stage('Test') {
            steps {
                sh 'mvn test'
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