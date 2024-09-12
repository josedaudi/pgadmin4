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

//         stage("Install Dependencies") {
//             steps{
//                 script {
//                     installPIPRequirements 'requirements.txt'
//                 }
//             }
//         }

        stage("Build Docker Image") {
            steps{
                script {
                    buildDockerImage "nexus.encipher.io:8083/sphere-link-africa-pgAdmin4:$env.BUILD_NUMBER"
                }
            }
        }

        stage("Push Docker Image to Nexus") {
            steps{
                script {
                    dockerLogin 'nexus.encipher.io:8083'
                    pushDockerImage "nexus.encipher.io:8083/sphere-link-africa-pgAdmin4:$env.BUILD_NUMBER"
                }
            }
        }
    }

}