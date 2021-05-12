@Library('pipeline-common@master') _

pipeline {
    environment {
        NODE_ENV = 'test'
        NODE_VERSION = '14'
        NPM_TOKEN = credentials('jenkins-checkit-npm-token')
    }
    agent {
        node {
            label 'ec2-node8'
        }
    }
    stages {
        stage('Audit') {
            steps {
                yarnAudit nvmVersion: env.NODE_VERSION, level: 'high'
            }
        }
        stage('Build') {
            steps {
                buildYarn nvmVersion: env.NODE_VERSION
            }
        }
        stage('Publish') {
            when {
                branch 'master'
            }
            steps {
                publishNpmModule nvmVersion: env.NODE_VERSION
            }
        }
    }
    post {
        failure {
            script {
                onFailure environment: env
            }
        }
    }
}
