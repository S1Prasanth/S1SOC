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
                sh 's1-cns-cli config --service-user-api-token $TOKEN --management-console-url https://usea1-purple.sentinelone.net --scope-type ACCOUNT --scope-id 1878467188785021855 --tag code:code'
            
                sh 's1-cns-cli  scan secret -d $WORKSPACE --pull-request origin/$SOURCE_BRANCH  origin/$DESTINATION_BRANCH'
            }
        }
    }
}
