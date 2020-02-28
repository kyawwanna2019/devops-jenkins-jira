pipeline {
    
    agent any
    tools { 
        //maven 'M2_HOME' 
        jdk 'JAVA_HOME' 
        gradle 'gradle-6.2.1'
    }

    parameters { 
        string(defaultValue: "https://github.com/kyawwanna2019/hello-world-2.git", description: 'Whats the github URL?', name: 'URL')
    }


    
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "${params.URL}"
            }
        }

        stage ('Build') {
            steps {
                //sh "mvn clean install package"
                sh "tool name: 'gradle-6.2.1', type: 'hudson.plugins.gradle.GradleInstallation'/bin/gradle clean build --info 2>&1 | tee gradle.build.${BUILD_NUMBER}.log"
                //sh "ls -la build/libs/*.war"
            }
        }

        stage ('Deploy') {
            steps {
                sshagent(['tomcat-dev']) {
                    //sh "scp -o StrictHostKeyChecking=no webapp/target/*.war ec2-user@18.236.84.72:~/Tomcat/webapps"
                    sh "scp -o StrictHostKeyChecking=no webapp/target/*.war ec2-user@18.236.84.72:~/Tomcat/webapps"
                }
            }
        }

        stage('Raise JiraIssue') {

            steps {
                 script {
                    def issue = [fields: [ project: [key: 'TPRO'],
                                summary: 'New JIRA Created from Jenkins.',
                                description: 'New JIRA Created from Jenkins.',
                                issuetype: [name: 'Task']]]
                    
                    def newIssue = jiraNewIssue issue: issue, site: 'Jira'
                    
                    def newIssueId = newIssue.data.key
                    echo newIssueId
                    
                    def attachment1 = jiraUploadAttachment site: 'jira', idOrKey: newIssueId, file: "gradle.build.${BUILD_NUMBER}.log"
                }
            }

           
        }
    }

}