node() {

    try {

       stage('Checkout'){

          checkout scm
       }

       stage('NPM Install'){
         sh 'node -v'
         dir('users-service') {
		sh "npm install"
		 }
       }
       stage('NPM Unit Test'){
		 sh 'npm test'
       }

       stage('Container Build'){
		 sh "docker-compose build --force"
       }

	   stage('Container Deploy'){
		 sh "docker-compose up -d"
       }
	   
	   stage('Node Integration Testing'){
		 dir('integration-test') {
			sh "npm install && npm start"
		 }
       }
	   
       stage('Cleanup'){
	     sh 'npm prune'
		 sh 'npm cache clean'
         sh 'rm node_modules -rf'
		 sh 'docker-compose down'

         mail body: 'project build successful',
                     from: 'jenkins@buildserver.com',
                     replyTo: 'anuj_sharma401@yahoo.com',
                     subject: 'project build successful',
                     to: 'anuj_sharma401@yahoo.com'
       }



    }
    catch (err) {
        currentBuild.result = "FAILURE"
            mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'jenkins@buildserver.com',
            replyTo: 'anuj_sharma401@yahoo.com',
            subject: 'project build failed',
            to: 'anuj_sharma401@yahoo.com'

        throw err
    }

}
