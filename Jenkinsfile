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
     }
}     