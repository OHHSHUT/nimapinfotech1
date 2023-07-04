pipeline {
    agent any
 environment {
        PATH = "$PATH:/usr/local/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Sayali-shipurkar/nimapinfotech1.git'
            }
        }

        stage('Build and Deploy') {
            steps {
                sh 'docker-compose  -f docker-compose.yaml up -d'
                
                // Get the container ID of the running application
                script {
                    def containerId = sh(returnStdout: true, script: "docker-compose ps -q fastapiwalacontainer").trim()
                    env.CONTAINER_ID = containerId
                }
            }
        }

        stage('Configure Prometheus') {
            steps {
                // Copy Prometheus configuration file to the container
                sh "docker cp prometheus.yml ${env.CONTAINER_ID}:/etc/prometheus/prometheus.yml"
            }
        }

        stage('Configure Grafana') {
            steps {
                // Copy Grafana provisioning files to the container
                sh "docker cp datasource.yml ${env.CONTAINER_ID}:/etc/grafana/provisioning/datasources/datasource.yml"
                sh "docker cp dashboard.yml ${env.CONTAINER_ID}:/etc/grafana/provisioning/dashboards/dashboard.yml"
            }
        }
    }
}
