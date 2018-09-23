node{
    
    def mvnHome = tool 'M3'
    stage('Checkout SCM'){
        
        //git 'https://github.com/Rahulmahadev/Maven_Desktop.git'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Rahulmahadev/Maven_Web.git']]])
        }
    stage('Build'){
        
        sh "'${mvnHome}'/bin/mvn clean package"
    }
    stage('Test'){
        echo "Im in TEst"
        // sh "'${mvnHome}'/bin/mvn test"
    }
    stage('Result'){
       //junit 'target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.war'
    }
     stage('SonarQube Analysis'){
    
        withSonarQubeEnv('mySonar') {
            sh "'${mvnHome}'/bin/mvn sonar:sonar"
        }
    }
    stage('publish nexus'){
        
       // nexusPublisher nexusInstanceId: '1234', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'my-app-web.war']], mavenCoordinate: [artifactId: 'my-app-web', groupId: 'com.mycompany.app', packaging: 'war', version: '9.9']]]
  nexusPublisher nexusInstanceId: '1234', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/my-app-web.war']], mavenCoordinate: [artifactId: 'my-app-web', groupId: 'com.mycompany.app', packaging: 'war', version: '11.5']]]

    }
    stage('Deploy Tomcat'){
        sh 'sudo cp target/*.war /var/lib/tomcat8/webapps'
    }
    stage('Email Notification')
    {
        mail bcc: '', body: 'Jenkins Job Successfully Completed', cc: '', from: '', replyTo: '', subject: 'Jenkins Job Successfully Completed', to: 'rahul.kulkarni59@gmail.com'

    }
}
