pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "rahul4884/train-schedule"
    }
    stages {
        stage("Checkout from github repo"){
            steps{
            git url: 'https://github.com/mayorpasca32/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            steps {
                kubeconfig(caCertificate: 'MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5pa3ViZUNBMB4XDTI0MDYzMDE2MDk0MloXDTM0MDYyOTE2MDk0MlowFTETMBEGA1UEAxMKbWluaWt1YmVDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALfAqIMS3sCBUftJgdrCsT/sC2YF0bC29i62VQtgshP6ZhKdwCzQ9UfjyUAn6in+xQlpDSBMA9xiCJTDnqI2dHwGSZ0XNz0RZ07OJSyWul6fbSQUUy4jQ2FfC7hTNbcWmx747n8kGpsXmTYYp/iZZL1Ar/8DA0xn2OHIsnEYiIREH7uKOVwcIIh1S5iv9eeA6ZG1QQBT5Wm3YFRP3FwMS2rzue4NRfWChp/RJ5TXpiZU1Urgekj/iVDeAo2n6NIlFu8zcVKAVyVpRXKkvUs6Zo108dMfma2zpDSvsvVY6XjDRCXdZ6kObD0y7rNfvMMggHGuJM5DuYInjIBX6BDLDdsCAwEAAaNhMF8wDgYDVR0PAQH/BAQDAgKkMB0GA1UdJQQWMBQGCCsGAQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBQr/9eOrN7k0v4bmhrF+VaAX4eUQzANBgkqhkiG9w0BAQsFAAOCAQEAalxuWMQPtrQCCwluVAMK0Jr9zKAqGo6FvEqfODuaXSF08AI+vN+UasKA428LDwwI0jHXsXAy+6bjGfjVznBW9k5Kr60TLcQxs32SP94QLW+LNAKjHe61vA3/0N9sqA2afPZuUQ/utUhgCBZnspvKWkO5rZVC16+tFRlkJQRwujoNk2/h6BDIDDiygb04t5GWwjPHVgPZsOuBGuOa6zj0YDYugITQ4VNAqR0Ih17HULqZ5J/0X/uUD2d6BO1lfH6KNqVuqKlbWqi4Iwy806260MwrnKAWMCxq4L/DfOaJuQ43QZ8HmN+yQQJ+qGOMaBA8QfOq7QKeLISa7CkgeWN3XA==', credentialsId: 'kubernetes', serverUrl: 'https://127.0.0.1:32781') {
    // some block
                 sh 'kubectl apply -f deployment.yaml'
                 sh 'kubectl apply -f app-service.yaml'
                 sh 'kubectl rollout restart deployment train-schedule'
}
            }
        }
    }
}
