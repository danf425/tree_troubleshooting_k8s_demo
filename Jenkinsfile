#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage ('hello world') {
            steps {
                echo "hello world"
            }
        }
        stage ('Deploy new YAML into production') {
            steps{
                sshagent(credentials : ['danf-ubuntu-k8s']) {
                    sh """#!/bin/bash
                    ssh -o StrictHostKeyChecking=no  ubuntu@18.206.87.22 << EOF
                    uptime
                    [ ! -d "/home/ubuntu/tree_troubleshooting_k8s_demo" ] && echo "GitHub repo doesn't exists. Cloning..." && git clone https://github.com/danf425/tree_troubleshooting_k8s_demo.git
                    cd tree_troubleshooting_k8s_demo
                    git pull 
                    # [ ! \$(docker images -q dmontanez/logdna-kubecon-demo) ] && echo "Docker image does not exist. Building..." && docker build . -t dmontanez/logdna-kubecon-demo
                    kubectl apply -f app.yaml
                    cd ..
                    echo "here are my problems"
                    sleep 1
                    echo "curl"
                    echo "curl" > temp.txt
                    kubectl get svc tree-lb-service -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}' >> temp.txt
                    echo ":8080" >> temp.txt
                    cat temp.txt
                    tr -d '\n' < temp.txt > temp2.txt
                    cat temp2.txt
                    # echo "\$(kubectl get svc tree-lb-service -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'):8080"
                    # curl "\$(kubectl get svc tree-lb-service -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'):8080"
                    # svc_url="\$(kubectl get svc tree-lb-service -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}')"
                    # svc_url+=":8080"
                    # echo \$svc_url
                    # sleep 5
                    # curl \$svc_url                    
                    """
                }

            }
        }
    }
}