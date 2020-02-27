def GRADLE_HOME = tool name: 'gradle-4.10.2', type: 'hudson.plugins.gradle.GradleInstallation'
def REPO_URL = 'https://github.com/kyawwanna2019/hello-world-2.git'
def REPO_BRANCH = 'branch2'
def DOCKERHUB_REPO = 'kyawwanna/java-webapp'

def TOMCAT_USER = 'admin'
def TOMCAT_PASSWORD = 'admin'
def WAR_PATH = 'webapp/target/*.war'
def TOMCAT_HOST = '34.210.99.226'
def TOMCAT_PORT = '8080'

pipeline {
    
    agent any
    tools { 
        maven 'M2_HOME' 
        jdk 'JAVA_HOME' 
    }

    // def JIRA_SITE_NAME = 'jira'
    // def JIRA_PROJ_NAME = 'SKYNET'
    
    


    stages {
        stage ('Clone') {
            steps {
                git branch: REPO_BRANCH, url: REPO_URL
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
                    sh "scp -o StrictHostKeyChecking=no ${WAR_PATH} ec2-user@${TOMCAT_HOST}:~/Tomcat/webapps"
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