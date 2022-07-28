node{
    stage('Scm Checkout'){
        git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/bargavsiripurapu/flask_demo_Latest.git'
    }
    stage('Mvn Package'){
        def mvnHome = tool name: 'maven-3', type: 'maven'
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage("SonarQube Analysis"){
        def mvnHome = tool name: 'maven-3', type: 'maven'
        withSonarQubeEnv('sonar_hub'){
            sh "${mvnHome}/bin/mvn sonar:sonar"
        }
    }
    stage('Build Docker Image'){
        sh 'docker build -t devopshub123/flask_demo_latest:2.0.0 .'
    }
    stage('Push image') {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
        sh "docker push devopshub123/flask_demo_latest:2.0.0"
        }
    }
    stage('login server'){
        sshagent(['root_ssh_key']){
            sh "ssh -o StrictHostKeyChecking=no root@13.127.209.175 'whoami'"
            sh "ssh -o StrictHostKeyChecking=no root@13.127.209.175 'sudo yum update'"
            sh "ssh -o StrictHostKeyChecking=no root@13.127.209.175 'ls -l'"
            sh "ssh -o StrictHostKeyChecking=no root@13.127.209.175 'docker pull devopshub123/flask_demo_latest:2.0.0'"
            sh "ssh -o StrictHostKeyChecking=no root@13.127.209.175 'docker run -d -p 80:8080 --name=mydemo5 devopshub123/flask_demo_latest:2.0.0'"
            
        }
    }
}