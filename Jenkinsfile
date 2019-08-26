pipeline {
    agent {
        label 'java'
    }
    
    stages {
        
        stage('Clean') {
            steps {
                sh 'mvn -B -DskipTests clean'
            }
        }
        stage('Test') {
            steps {
		            sh 'mvn compile'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Static Analysis for Sonar') {
            steps {
                withSonarQubeEnv('YSBSonarqube') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }
        stage('Sonar Gate') {
            steps {
                sh 'echo "sonar gate ok"'
            } 
        }
        stage('Build & Publish') {
            steps {
                sh 'mvn install package'
		       } 
        }
        stage('Save to tmp') {
            steps {
                sh 'mkdir -p /tmp/location_for_dashboard_artifacts; cd /tmp/location_for_dashboard_artifacts; cp *.jar /tmp/location_for_dashboard_artifacts'
            }        
        }
    }
}
