node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImage
    def dockerImageTag = "devopsexample${env.Build_NUMBER}"

    stage('Clone repo') {
        git url: 'https://github.com/KillianMalon/JenkinsTest.git'
    }

    stage('Build project') {
        sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
    }

    stage('Initialize Docker') {
        def dockerHome = tool 'MyDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }

    stage('Build Docker Image') {
        sh "docker -H tcp://192.168.133.134:8080 build -t devopsexample:${env.BUILD_NUMBER} ."
    }
    
    stage('Deploy DOcker Image'){
        echo "Docker Image Tag Name: ${dockerImageTag}"
        sh "docker -H tcp://192.168.133.134:8080 run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"
    }
}
