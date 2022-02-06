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
                    status = `curl -I -s localhost:9889|head -n1|cut -d " " -f 2`
                    sh "echo ${status}"
                    if (status == '200') {
                        currentBuild.result = "SUCCESS"
                    }
                    else {
                        currentBuild.result = "FAILURE"
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
