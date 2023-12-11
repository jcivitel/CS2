pipeline{
	agent any
	options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }
	stages{
		stage('Building image') {
			steps{
				script{
					docker.withRegistry('','dockerhub') {
						def myImage = docker.build("jcivitell/cs2:${env.BUILD_ID}", "./bullseye")
						myImage.push()
            myImage.push('latest')
					}
				}
			}
		}
	}
	post{
	    cleanup{
            cleanWs()
        }
	}
}
