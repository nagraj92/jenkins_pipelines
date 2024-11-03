pipeline{
agent any
    environment{
    def appVersion = ''
    }
    stages{
        stage('read the version'){
        steps{
        script{
            def packageJson = readJSON file: 'package.json'
            appVersion= packageJson.version
            echo "applicationVersion: $appVersion"
        }
        }
        }
      stage('build'){
        steps{
            sh """
            zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
            ls -ltr
            """
        }
      }
        stage('nexus artifact upload'){
          steps{
              script{
                 nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: 'http://192.168.56.1:8081',
                            groupId: 'com.expensee',
                            version:  "${appVersion}",
                            repository: 'backend',
                            credentialsId: 'nexus_auth',
                            artifacts: [
                                [artifactId: "backend",
                                 classifier: '',
                                 file: 'backend-' +"${appVersion}" + '.zip',
                                 type: 'zip']
        ]
     )

              }
          }
        }
        
    }



}
