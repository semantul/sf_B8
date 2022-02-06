pipeline {
    agent {
        docker {
            image 'nginx:latest'
            args '-v /var/lib/jenkins/workspace/Test_pipeline:/usr/share/nginx/html -p 9889:80'
        }
    }
    stages {
        stage('Test') {
            agent any
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
    }
}
