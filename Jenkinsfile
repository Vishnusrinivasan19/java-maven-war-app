pipeline{
    agent any

    tools{
        maven'Maven_3.6.0'
    }

    stages{
        stage('SCM Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Vishnusrinivasan19/java-maven-war-app.git']])
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Sonar Scan'){
            steps{
                withSonarQubeEnv("SonarQube"){
                sh "${tool("SonarQube_4.7.0.2747")}/bin/sonar-scanner \
                -Dsonar.host.url=http://ec2-13-233-158-248.ap-south-1.compute.amazonaws.com:9000// \
                -Dsonar.login=sqp_1fea6fe4bce62a1917e4b5bdb59c06ca435520dd \
                -Dsonar.projectKey=java-maven-war-app \
                -Dsonar.java.binaries=target/"
                }
            }
        }
        stage("Nexus Upload"){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }
}
