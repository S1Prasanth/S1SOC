pipeline {
    agent none
    stages {
        stage('Configure and Scan') {
            agent {
                docker {
                    image 'pingsafe/s1-shift-left-cli:0.4.7'
                    args '--entrypoint=' // Ensures the default entrypoint is not used, allowing direct command execution
                }
            }
            environment {
                TOKEN = credentials('S1_SERVICE_USER_API_TOKEN')
                HOME = "/tmp"
            }
            steps {
                script {
                    sh 'git checkout origin/$SOURCE_BRANCH'
                    echo 'Starting s1-cns-cli'
                    
                    sh 's1-cns-cli config --service-user-api-token $TOKEN --management-console-url https://usea1-purple.sentinelone.net --scope-type ACCOUNT --scope-id 1878467188785021855 --tag code:code'
                    
                    def secretScanStatus = sh(script: 's1-cns-cli scan secret -d $WORKSPACE --pull-request origin/$SOURCE_BRANCH origin/$DESTINATION_BRANCH', returnStatus: true)
                    def iacScanStatus = sh(script: 's1-cns-cli scan iac -d $WORKSPACE', returnStatus: true)
                    def vulnScanStatus = sh(script: 's1-cns-cli scan vuln -d $WORKSPACE', returnStatus: true)
                    
                    if (secretScanStatus != 0 || iacScanStatus != 0 || vulnScanStatus != 0) {
                         error('One or more s1-cns-cli scans failed. Check logs for details.')
                    }
                }
            }
        }
    }
}
