pipeline {
    agent any
    stages {
        stage('Build') {
          when {
            changeset "index.html"
          }
          steps {
              sh 'docker run --rm -d -v /var/lib/jenkins/workspace/Test-pipeline:/usr/share/nginx/html -p 9889:80 nginx:latest'
          }
	      }
        stage('Test_code') {
            when {
              changeset "index.html"
            }
            steps {
                    sh 'curl -Is http://localhost:9889|head -n1|cut -d " " -f2|grep -q 200 && echo true || exit 1'
            }
        }
        stage('Test_checksum') {
            when {
              changeset "index.html"
            }
            steps {
                sh 'if (( `md5sum index.html|cut -d " " -f1` == `curl -s http://localhost:9889|md5sum|cut -d " " -f1` )); then echo true; else exit 1;fi'
            }
        }
    }
    post {
        always {
          sh 'docker stop `docker ps|grep 9889|cut -d " " -f1`'
        }
        failure {
            mail body: "The Pipeline Test-pipeline is failed", to: "semantul@yandex.ru", subject: "Jenkins Notification"
        }
    }
}
