#!/usr/bin/env groovy
@Library('jenkins-shared-library')_

pipeline {

    agent any

    stages{

        stage("Create Virtual ENV") {
            steps{
                script {
                    createPythonVENV()
                }
            }
        }

        stage("Build Docker Image") {
            steps{
                script {
                    buildDockerImage "nexus.sphere.tz/sphere-link-africa-pgadmin4:$env.BUILD_NUMBER"
                }
            }
        }

        stage("Push Docker Image to Nexus") {
            steps{
                script {
                    dockerLogin 'nexus.sphere.tz'
                    pushDockerImage "nexus.sphere.tz/sphere-link-africa-pgadmin4:$env.BUILD_NUMBER"
                }
            }
        }
    }

}