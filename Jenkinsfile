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
       
      }
    }

   stage ('Deploy to Staging'){
	when {
		branch 'master'
	}
	echo 'Inside deployment'
	steps {
	echo 'Inside steps'
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'srcHelloWorld/HelloWorld.zip',
                                        removePrefix: 'srcHelloWorld/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'unzip /tmp/HelloWorld.zip -d /opt/HelloWorld'
                                    )
                                ]
                            )
                        ]
                    )
echo 'Outside'
                }
  }
}
}
}
