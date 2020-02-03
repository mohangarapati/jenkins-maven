pipeline {
   agent any
   triggers {
    pollSCM '* * * * *'
   }
   rtServer (
       id: 'Artifactory-1',
       url: 'http://104.198.202.49/artifactory/',
       credentialsId: 'artifactory'
       timeout = 300
   )
   
    stages {
      stage('checkout') {
         steps {
            // Get some code from a GitHub repository
            //git 'https://github.com/mohangarapati/docker-jenkins-poc.git'
            
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'c76de21c-3533-47d8-bf8f-102f4a4fe2f8', url: 'https://github.com/mohangarapati/simple-maven.git']]])
            }
      }
      stage('Build') {
        steps {
		sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven/bin/mvn -f pom.xml clean install'
	            }
     	   }
        stage('Test') {
            steps {
                sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven/bin/mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
      
      stage('upload to artifactory') {
            steps {
		rtDownload (
   		serverId: 'Artifactory-1',
    		spec: '''{
          "files": [
            {
              "pattern": "maven/",
              "target": "bazinga/",
            }
          ]
	    }''',
 
	)
            }
        }
    }  
} 
