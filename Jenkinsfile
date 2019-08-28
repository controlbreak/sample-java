pipeline {
    agent any
    tools {
        jdk 'jdk8'
        maven 'maven3'
    }
    stages {
        stage('install and sonar parallel') {
            steps {
                parallel(install: {
                    sh "mvn -U clean test cobertura:cobertura -Dcobertura.report.format=xml"
                }, sonar: {
                    sh "mvn -U clean test sonar:sonar -Dsonar.host.url=${env.SONARQUBE_HOST} \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.tests=src/test/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.java.test.binaries=target/test-classes \
                        -Dsonar.junit.reportsPath=target/surefire-reports \
                        -Dsonar.surefire.reportsPath=target/surefire-reports \
                        -Dsonar.jacoco.reportPaths=target/jacoco.exec \
                        -Dsonar.java.coveragePlugin=jacoco"
                })
            }
            post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'
                    step([$class: 'CoberturaPublisher', coberturaReportFile: 'target/site/cobertura/coverage.xml'])
                }
            }
        }
    }
}
