pipeline {
    agent any

    tools {
        // Install the Maven version configured as "Mavn" and add it to the path.
        maven "Maven"
    }

    stages {
        stage('SCM Checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'anil135git', url: 'https://github.com/anil135/TomcatMavenApp.git']]])
        }
        }
        stage('Build') {
            steps {
              
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

            }

            post {
                // If Maven was able to run the tests, record the test results and archive the war file
                success {
                    archiveArtifacts 'target/*.war'
                }
            }
        }
        stage('Deploy'){
			steps {
			    //Deploys the war file in tomcat container.
				deploy adapters: [tomcat9(credentialsId: 'Manager', path: '', url: '172.26.93.249:8000/')], contextPath: null, war: '**/*war'
        }
		}
		stage('Show http status')
        {   steps{
                sh 'curl -I "http://172.26.80.73:8000/TomcatMavenApp/" | grep HTTP | head -1'
            }
        }
    }
}
