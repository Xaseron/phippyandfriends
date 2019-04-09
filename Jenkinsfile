pipeline {
    options {
        timestamps()
    }
    agent {
        node {
            label 'master'
        }
    }
    environment {
        /* get the foo out of e.g. foo/master */
        MICROSERVICE = "${env.JOB_NAME.tokenize('/')[0]}"
    }
    stages {
        /* useless stage to get the when block at the top level */
        stage('Meta') {
            /* only start a build when something changed inside the microservice or it is triggered manually */
            when {
                anyOf {
                    triggeredBy cause: 'UserIdCause'
                    changeset "$MICROSERVICE/**"
                    changeset 'Jenkinsfile'
                }
            }
            stages {
                stage('Build') {
                    steps {
                        echo "I've got a change in $MICROSERVICE"
                        echo "docker build -t $MICROSERVICE:$BUILD_NUMBER $MICROSERVICE"
                    }
                }
                stage('Test') {
                    steps {
                        echo 'test'
                    }
                }
                stage('Deploy') {
                    steps {
                        echo 'deploy'
                    }
                }
            }
        }
    }
}
