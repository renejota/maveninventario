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
        
     }
     
     post {
}     