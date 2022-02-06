pipeline {
    agent any
    stages {
	     stage('Build') {
          steps {
              sh """
                  docker run --rm -d -v /var/lib/jenkins/workspace/Test_pipeline:/usr/share/nginx/html -p 9889:80 nginx:latest
              """
          }
	}
        stage('Test') {
            steps {
                echo 'Testing...'
                sh """
                    echo "${env.WORKSPACE}"
                    ss -nlpt
                    pwd
                    ls -la
                    curl -I localhost:9889
                """
            }
        }
        stage('Post_test') {
            steps {
              sh """
                  docker stop $(docker ps|grep 9889|awk '{print $1}')
              """
            }
        }
    }
}
