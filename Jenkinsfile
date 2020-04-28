pipeline
{
  agent any									//to run on any agent
  stages{
    stage ('Build' ){
	when {
                branch 'master'
            }
      steps {
        echo 'Running Build Automation'
        sh './gradlew build'
	archiveArtifacts artifacts: 'srcHelloWorld/HelloWorld.zip'
       
      }
    }

   stage ('Deploy to Staging'){
	when {
		branch 'master'
	}
	steps {
	echo 'Inside steps'
                withCredentials([usernamePassword(credentialsId: 'deployment_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deployment',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: '/tmp/srcHelloWorld/HelloWorld.zip',
                                        removePrefix: 'tmp/srcHelloWorld/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'unzip /tmp/HelloWorld.zip -d ./HelloWorld'
                                    )
                                ]
                            )
                        ]
                    )
                }
  }
}
}
}
