pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker { 
                    image 'pingsafe/s1-shift-left-cli:0.4.7'
                    args '--entrypoint='
                 }
            }
            environment {
                TOKEN = credentials('S1_SERVICE_USER_API_TOKEN')
                HOME = "/tmp"
            }
            
            steps {
                echo 'Starting s1-cns-cli'
                sh 's1-cns-cli config --service-user-api-token $TOKEN --management-console-url <mgmt-console-url> --scope-type <scope-type> --scope-id <scope-id> --tag <scanner-policy-tag>'
            
                sh 's1-cns-cli  scan secret -d $WORKSPACE --pull-request origin/$SOURCE_BRANCH  origin/$DESTINATION_BRANCH
            }
        }
    }
}
