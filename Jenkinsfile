pipeline {
    agent any
    tools{
        nodejs "18.1.0"
    }
    stages {
        stage("npm-login-cli"){
        steps{
        sh 'npm -v'
        }
        }
        stage("build") {
            steps {
                echo "building.." 
				   sh 'docker build . -f docker1.dockerfile -t builder'
            }
        }
        stage('test') {
            steps {
			  sh 'docker build . -f docker2.dockerfile -t tester'
			  sh' docker run tester'
            }
        }
         stage('deploy') {
            steps {
               echo 'deploying..'
                 sh  'docker build . -f docker3.dockerfile -t deploy'
				 sh 'docker stop $(docker ps -a -q)'
				 sh 'docker run  -d -p 3000:80 deploy '		                 
            }
        }
        stage('publish') {
            steps {
               echo "publishing.."
			    sh 'docker build . -f docker4.dockerfile -t publish'
				sh "docker run --volume /var/jenkins_home/workspace/volumes:/final publish mv nodejs.tar.xz /final"
				echo "NODEJS.ORG VERSION ${params.version} HAS BEEN PUBLISHED" 	

            }
        }
    }
}
