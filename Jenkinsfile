node {
    def app

    stage('Checkout Code') {
      

        checkout scm
    }

    stage('Build Image') {
  
       app = docker.build("joeygeofrey/fblog")
    }

    stage('Test Image') {

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push Image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Update Manifest') {
                echo "triggering updatemanifestjob"
                build job: 'FlaskBlog_CD', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
