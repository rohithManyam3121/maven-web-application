node
{
    def mavenHome = tool name: "maven3.8.6"
    
    echo "the job name is : ${env.JOB_NAME}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    stage('CheckoutCode'){
        git branch: 'development', url: 'https://github.com/rohithManyam3121/maven-web-application.git'
    }
    stage('build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('executeSonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('uploadArtifactIntoNexus'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('deployAppIntoTomcatServer'){
        sshagent(['914c2a02-b8b0-4fc7-ba2a-c93f9f02aa60']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.177.181.146:/opt/apache-tomcat-9.0.64/webapps/"
}
        
    }
        
}
