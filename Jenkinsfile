pipeline {
    agent {
        docker {
            image 'maven:3-jdk-8' 
            args '-v /root/.m2:/root/.m2 --network jenkins_default' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                wrap([$class: 'VeracodeInteractiveBuildWrapper', location: 'veracode-iast', port: '10010']) {
                    sh 'echo $IASTAGENT_LOGGING_STDERR_LEVEL'
                    sh 'curl -sSL https://s3.us-east-2.amazonaws.com/app.veracode-iast.io/iast-ci.sh | DEBUG=1 sh'
                    sh 'mvn --debug test'
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh 'echo mvn install would run here...'
            }
        }
    }
}
