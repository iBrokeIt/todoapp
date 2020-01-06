node {
   def commit_id
   stage('step a') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = "bobcat"
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
      sh label: '', script: '''
      npm install
      npm test 
      '''
     }
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'docker-frenzy669') {
       def app = docker.build("frenzy669/docker-nodejs-demo:${commit_id}", '.').push()
     }
   }
   stage('docker run') {
     sh label: '', script: """
      docker run --rm -tid --name docker_test -p 3000 frenzy669/docker-nodejs-demo:${commit_id}
      docker kill docker_test
      """
     }
   }

