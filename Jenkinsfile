pipeline {
  agent { label 'linux' }
    stages {
      stage('initial') {
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                    doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
                    userRemoteConfigs: 
                    [[credentialsId: 'gitHubCreds', url: 'https://github.com/gideonaina/wordCount.git']]])
          
            sh 'cd wordCount && ls -la'
        }
      }
      stage('Creds') {
        steps {
          sh 'echo "%%%%%%%%%%CREDS%%%%%%%%%%%"'
    }
}

      stage('Test') {
        steps {

          sh 'echo "***************TEST*****************"'
          withCredentials([usernamePassword(credentialsId: 'restEndpoint', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh returnStdout: true, script: '''
            echo "-Puname=$USERNAME -Dpass=$PASSWORD"
            cd wordCount && ls -la && chmod +x gradlew && ./gradlew --refresh-dependencies && ./gradlew clean build &&  ./gradlew test -Ppflag="access token" -Duname=$USERNAME -Dpass=$PASSWORD
            '''
          }
        }
     }
      
       stage('Report') {
        steps {

          sh 'echo "***************REPORT*****************"'
           sh 'cd wordCount && ls -la build/reports/tests/**'
          //sh 'cd wordCount && ls -la build/test-results/**'
          junit '**/test-results/test/*.xml'
        }
     }
      
  }
}
