pipeline {
    agent any
    stages {
	     stage('Build') {
          steps {
              sh 'docker run --rm -d -v /var/lib/jenkins/workspace/Test-pipeline:/usr/share/nginx/html -p 9889:80 nginx:latest'
          }
	      }
        stage('Test_code') {
            steps {
                    sh 'curl -Is http://localhost:9889|head -n1|cut -d " " -f2|grep -q 200 && echo true || exit 1'
            }
        }
        stage('Test_checksum') {
            steps {
                sh 'if (( `md5sum index.html|cut -d " " -f1` == `curl -s http://localhost:9889|md5sum|cut -d " " -f1` )); then echo true; else exit 1;fi'
            }
        }
        stage('Post_test') {
            steps {
                sh 'docker stop `docker ps|grep 9889|cut -d " " -f1`'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'Failure Test-pipeline'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
