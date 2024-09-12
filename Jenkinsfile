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

        stage("Install Dependencies") {
            steps{
                script {
                    installPIPRequirements 'requirements.txt'
                }
            }
        }

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

        stage("Deploy APP in K8S") {
            steps{
                script {
                    echo "Deploying APP to Kubernetes Self Hosted..."

                    withKubeConfig(caCertificate: '', clusterName: 'default', contextName: '', credentialsId: 'kubernetes-cluster-role-credentials', namespace: 'sphere-link-management-tools', restrictKubeConfigAccess: false, serverUrl: 'https://rancher.encipher.io:6443') {
                        sh "kubectl apply -f k8s-deployment/app/"
                    }
                }
            }
        }

        stage("Verify K8S Deployment") {
            steps{
                script {
                    echo "Verifying Deployment in Kubernetes Self Hosted..."

                    withKubeConfig(caCertificate: '', clusterName: 'default', contextName: '', credentialsId: 'kubernetes-cluster-role-credentials', namespace: 'sphere-link-management-tools', restrictKubeConfigAccess: false, serverUrl: 'https://rancher.encipher.io:6443') {
                        sh "kubectl get pods -n sphere-link-management-tools"
                        sh "kubectl get svc -n sphere-link-management-tools"
                    }
                }
            }
        }
    }

}