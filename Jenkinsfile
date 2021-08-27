node{

	def mavenHome = tool name: "maven-3.8.2"

	stage('checkoutCode')
	{
		git branch: 'development', url: 'https://github.com/Sahana-MK/maven-web-application.git'
	}

	stage('Build')
	{
		sh "${mavenHome}/bin/mvn clean package"
	}
	
	stage('sonarQubeReport')
	{
		sh "${mavenHome}/bin/mvn clean sonar:sonar"
	}
	
	stage('uploadArtifacttoNexus')
	{
		sh "${mavenHome}/bin/mvn clean deploy"
	}
	
	stage('deployAppintoTomcat')
	{
		sshagent(['39888fe0-e47b-4b47-b21f-0ca8b883da8b']) {
			sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline-scriptedway/target/maven-web-application.war ec2-user@13.232.207.33:/opt/apache-tomcat-9.0.52/webapps/"
		}
	}

	stage('sendEmailNotification')
	{
		emailext body: '''Build done

		Thanks''', subject: 'Build done', to: 'sahanabhat327@gmail.com'
	}
}
