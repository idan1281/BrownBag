
pipeline {
   agent any
   stages {
      def app
  
    stage('Clone repository') {
       steps{
           checkout scm
        }
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

         steps{
            app = docker.build("idan1281/brownbag")
        }
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */
         steps{
            app.inside {
                sh 'echo "Tests passed"'
            }
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
         steps{
             docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                 app.push("${env.BUILD_NUMBER}")
                 app.push("latest")
            }
        }
    }
}

