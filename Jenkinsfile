pipeline {
    agent any
    tools { 
        maven 'M2_HOME' 
        jdk 'JAVA_HOME' 
    }
    stages {
        stage ('Clone') {
            steps {
                git branch: 'branch2', url: "https://github.com/kyawwanna2019/hello-world-2.git"
            }
        }

        // stage ('Build') {
        //     steps {
        //         sh "mvn clean install package"
        //     }
        // }

        stage ('Build') {
            steps {
                sh "${GRADLE_HOME}/bin/gradle build --info 2>&1 | tee gradle.build.${BUILD_NUMBER}.log"
                sh "ls -la ${WAR_PATH}"
            }
        }

        stage ('Deploy') {
            steps {
                sshagent(['tomcat-dev']) {
                    sh 'scp -o StrictHostKeyChecking=no webapp/target/*.war ec2-user@34.210.99.226:~/Tomcat/webapps'
                }
            }
        }

        // stage('Raise JiraIssue') {
        //     def issue = [fields: [ project: [key: JIRA_PROJ_NAME],
        //                 summary: 'New JIRA Created from Jenkins.',
        //                 description: 'New JIRA Created from Jenkins.',
        //                 issuetype: [name: 'Task']]]
            
        //     def newIssue = jiraNewIssue issue: issue, site: JIRA_SITE_NAME
            
        //     def newIssueId = newIssue.data.key
        //     echo newIssueId
            
        //     def attachment1 = jiraUploadAttachment site: JIRA_SITE_NAME, idOrKey: newIssueId, file: "gradle.build.${BUILD_NUMBER}.log"
        // }
    }

}