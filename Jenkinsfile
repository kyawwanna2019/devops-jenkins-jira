
//START-OF-SCRIPT
node {
    def JIRA_SITE_NAME = 'jira'
    def JIRA_PROJ_NAME = 'SKYNET'
    
    def GRADLE_HOME = tool name: 'gradle-4.10.2', type: 'hudson.plugins.gradle.GradleInstallation'
    def REPO_URL = 'https://github.com/cloudacademy/devops-webapp.git'
    def DOCKERHUB_REPO = 'kyawwanna/java-webapp'

    /* Test for Tomcat Deployment */
    def mvnHome

    stage('Clone') {        
        git url: REPO_URL
        branch: 'master'
    }

    stage('Build') {
        sh "${GRADLE_HOME}/bin/gradle build --info 2>&1 | tee gradle.build.${BUILD_NUMBER}.log"
        sh "ls -la build/libs/*.war"
    }

    stage('Deploy') {
            //copyArtifacts(filter: '**/build/libs/*.war', flatten: true, projectName: 'MyProject', target: 'C:/www/MyProject', selector: specific('${BUILD_NUMBER}'))
            //sh "scp -o ScrictHostKeyChecking=no build/libs/*.war ec2-user@ec2-54-244-200-137.us-west-2.compute.amazonaws.com"
            sshPublisher(publishers: [sshPublisherDesc(configName: 'ec2-54-244-200-137.us-west-2.compute.amazonaws.com', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'apt-get update', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/home/ec2-user/Tomcat/webapps/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'build/libs/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }

//    stage ('Deploy'){
//    echo 'deployment started'
//        //bat '''copy C:\\Users\\Madhu\\.jenkins\\workspace\\kelly_pipeline_java_maven\\target\\*.war F:\\softwares\\apache-tomcat-7.0.53\\webapps\\'''
//        //sh "scp -o ScrictHostKeyChecking=no target/*.war ec2-user@ec2-54-185-228-143.us-west-2.compute.amazonaws.com"
//        sh "scp -i tomcat-keypair.pem http://localhost:8080/job/BuildJob2/4/execution/node/3/ws/build/libs/*.war ec2-user@ec2-54-185-228-143.us-west-2.compute.amazonaws.com"
//   }

    // stage('Raise JiraIssue') {
    //     def issue = [fields: [ project: [key: JIRA_PROJ_NAME],
    //                    summary: 'New JIRA Created from Jenkins.',
    //                    description: 'New JIRA Created from Jenkins.',
    //                    issuetype: [name: 'Task']]]
        
    //     def newIssue = jiraNewIssue issue: issue, site: JIRA_SITE_NAME
        
    //     def newIssueId = newIssue.data.key
    //     echo newIssueId
        
    //     def attachment1 = jiraUploadAttachment site: JIRA_SITE_NAME, idOrKey: newIssueId, file: "gradle.build.${BUILD_NUMBER}.log"
    // }
}
//END-OF-SCRIPT