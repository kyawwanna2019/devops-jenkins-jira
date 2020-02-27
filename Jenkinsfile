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

        stage ('Build') {
            steps {
                sh "mvn clean install package"
            }
        }

        stage ('Deploy') {
            steps {
                sshagent(['tomcat-dev']) {
                    sh 'scp -o StrictHostKeyChecking=no build/libs/*.war ec2-user@34.210.99.226:~/Tomcat/webapps'
                }
            }
        }
    }

     

}