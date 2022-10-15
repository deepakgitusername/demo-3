pipeline {
    agent {
        label 'linux2'
    }
    environment {
        KUBECONFIG_FILE = credentials('my-kubeconfig')
    }
    stages {
        stage('Verify Namespace') {
            steps {
                sh 'echo "Verifying Namespace"'
                sh '''
                kubectl --kubeconfig $KUBECONFIG_FILE get ns my_ns
                if [[ -$? == 0 ]]; then
                echo "Namespace exist"
                else
                kubectl --kubeconfig $KUBECONFIG_FILE create ns my_ns
                fi
                '''
            }
        }
        stage('Deploying Application') {
            steps {
                sh 'echo "Deploying Apps"'
                sh '''
                kubectl --kubeconfig $KUBECONFIG_FILE apply -f deployment.yaml my_ns
                kubectl --kubeconfig $KUBECONFIG_FILE get svc centos-apache
                if [[ -$? == 0 ]]; then
                echo "Service already exist"
                else
                kubectl --kubeconfig $KUBECONFIG_FILE expose deployment centos-apache --port=8080 --type=NodePort
                fi
                '''
            }
        }
    }
}