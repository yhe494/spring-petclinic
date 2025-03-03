pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1')  // Runs every 3 minutes on Mondays
    }

    tools {
        maven 'Maven 3'  
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 'https://github.com/yhe494/spring-petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Code Coverage with JaCoCo') {
            steps {
                sh 'mvn jacoco:prepare-agent test jacoco:report'
            }
            post {
                success {
                    jacoco(
                        execPattern: '**/target/jacoco.exec',
                        classPattern: '**/target/classes',
                        sourcePattern: '**/src/main/java',
                        exclusionPattern: '**/generated/**'
                    )
                }
            }
        }

        stage('Package Artifact') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
