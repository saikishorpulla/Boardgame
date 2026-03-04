pipeline {
    agent any 
    tools {
        maven 'maven_3.9.12'
        jdk 'jdk_17'
        sonarqube 'sonar_8.9'
        sonarScanner 'sonar-scanner_4.7.0'
        sonargate 'sonar-gate-4.0.0'
    }
    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        buildNumber = env.BUILD_NUMBER
        dockertag = env.BUILD_NUMBER
        }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        string(name:'GIT_URL', defaultValue:'https://github.com/saikishorpulla/Boardgame.git', description:'Git repository URL')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select the deployment environment')
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
        timeout(time: 30, unit: 'MINUTES')  
       }
       stages {
        stage('checkout') {
            when {
                expression { 
                    return params.BRANCH_NAME != '' && params.GIT_URL != '' 
                    }
            }
            steps {
                git branch: "${params.BRANCH_NAME}", url: "${params.GIT_URL}"
       }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

 }
}
