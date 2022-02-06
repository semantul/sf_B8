pipeline {
    agent any
    stages {
	     stage('Build') {
          steps {
              sh """
                  docker run --rm -d -v /var/lib/jenkins/workspace/Test-pipeline:/usr/share/nginx/html -p 9889:80 nginx:latest
              """
          }
	      }
        stage('Test') {
            steps {
                script {
                    echo 'Testing...'
                    def get = new URL("http://localhost:9889").openConnection();
                    def getRC = get.getResponseCode();
                    println(getRC);
                    if(getRC.equals(200)) {
                        println(get.getInputStream().getText());
                    }
                }
            }
        }
        stage('Post_test') {
            steps {
                sh """
                    docker stop `docker ps|grep 9889|cut -d " " -f1`
                """
            }
        }
    }
}
