pipeline {
    agent none

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"  
        jdk "java"
    }

    stages {
        stage('Compile') {
            agent any
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/edureka-git/DevOpsClassCodes.git/'

                // Run Maven on a Unix agent.
                sh "mvn compile"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }    
        }
        stage('Code_Review') {
            agent any
            steps {
                sh "mvn pmd:pmd"
            }
        }    
           
        stage('Unit_Test') {
            agent any
            steps {
                sh "mvn test"
            }
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }    
        stage('Metric_Check') {
            agent any
            steps {
                sh "mvn cobertura:cobertura -Dcobertura.report.format=xml"
            }
            post{
                always{
                    cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml '
                }
            }
        }
        
        stage('Package') {
            agent {label 'ubuntu_slave'}
            steps {
                git 'https://github.com/edureka-git/DevOpsClassCodes.git/'
                sh "mvn package"
            }
        }
    }
}
