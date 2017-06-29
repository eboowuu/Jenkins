pipeline {
    agent any
    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES') 
    }
    stages {
        stage('stage1') {
            steps {
                echo "stage 1 running...."
                echo "Success:Stage1"
            }
        }
   
        stage('Send email') {
            def mailRecipients = "eboowuu@gmail.com"
            def jobName = currentBuild.fullDisplayName
        
            emailext body: '''${SCRIPT, template="my-email.template"}''',
                subject: "[Jenkins] ${jobName}",
                to: "${mailRecipients}",
                replyTo: "${mailRecipients}",
                recipientProviders: [[$class: 'CulpritsRecipientProvider']]
        }
    }
    post {
        always {
            script{
                if(currentBuild.result == null) {
                    currentBuild.result = 'SUCCESS'
                }
                logs = currentBuild.rawBuild.getLog(200)
                email_logs = ""
                for (String item: logs) {
                    if(item.contains('[Pipeline]') == false) {
                        //item = item.replaceAll(/Bo Wu/, ' ')
                   
                        email_logs = email_logs + item + "\n"    
                        
                    }
                }
            }
            echo currentBuild.result
            emailext (
                attachLog: true,
                subject: "${currentBuild.result}: Job [${env.JOB_NAME}]",
                mimeType: 'text/html',
                body: """<p>${currentBuild.result}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                <h3 color='red'>Build Logs</h3>
                <pre color='red'>${email_logs}</pre>
                <p>Check console output at "<a href="${env.BUILD_URL}/consoleText">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
                to: 'eboowuu@gmail.com'
            )
        }
    }
}
