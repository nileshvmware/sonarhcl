
SONAR_PROJECT_KEY: enter sonar key
SONAR_CREDENTIALS_KEY: enter SONAR_CREDENTIALS_KEY,Sonar token that is used to connect and upload to Sonar Server
SONAR_MAIN_BRANCH: choose master SONAR_MAIN_BRANCH

Input Paramters
string SONAR_CREDENTIALS_KEY:XXXX
string SONAR_PROJECT_KEY:XXXX

Paramters
string{
        name: 'Branch_Name',
        description: 'Branch you want to build <br> Ex. master, release etc',
        trim: true
    }
    choice{
       name: 'Build_package'
       choices: {'no', 'yes'},
       description: 'Do you want to build Package?'

booleanParam{
    name: 'Run_SonarQube',
    defaultValue: false,
    description: 'click checkbox to run the Sonarqube scans'

pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
             stages {
          stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('My SonarQube Server') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        }
        }
    }
}



