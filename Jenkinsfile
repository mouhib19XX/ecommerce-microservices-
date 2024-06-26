def microservices = [
    'api-gateway',
    'cloud-config',
    'favourite-service',
    'order-service',
    'payment-service',
    'product-service',
    'proxy-client',
    'service-discovery',
    'shipping-service',
    'user-service'
]

pipeline {
    agent any

    tools {
        maven "maven"  // Define Maven tool version
    }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'  // Set the path to Java 17
        SCANNER_HOME = tool "SonarQube-Scanner"  // Set SonarQube scanner tool home directory
        APP_NAME = "ecommerce-microservices"
        RELEASE = "1.0.0"
        DOCKER_USER = "mouhib543"
        DOCKER_PASS = 'dockerhub'
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage('clean workspace') {
            steps {
                cleanWs()  // Clean workspace before starting
            }
        }
        
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/mouhib19XX/ecommerce-microservices-.git'  // Checkout code from Git
            }
        }
        
        stage ('Build Project') {
            steps {
                sh "./mvnw clean package"  // Build the project using Maven
            }
        }
        
        //stage('SonarQube Analysis') {
            //steps {
                //withSonarQubeEnv('SonarQube-Server') {
                    //sh './mvnw -U clean install sonar:sonar'  // Perform SonarQube analysis
                //}
            //}    
        //}
    
        //stage("Quality Gate") {
            //steps {
                //timeout(time: 1, unit: 'HOURS') {
                    //waitForQualityGate abortPipeline: true  // Wait for SonarQube Quality Gate result
                //}
            //}
        //}
        
        //stage ('Test Package') {
            //steps {
                //sh "./mvnw test"  // Run tests for the project
            //}
        //}
        
                //stage ('Artifactory configuration') {
            //steps {
                //rtServer (
                    //id: "jfrog-server",
                    //url: "http://localhost:8082/artifactory",
                    //credentialsId: "jfrog"
                //)  // Configure Artifactory server
                
                //rtMavenDeployer (
                    //id: "MAVEN_DEPLOYER",
                    //serverId: "jfrog-server",
                    //releaseRepo: "libs-release-local",
                    //snapshotRepo: "libs-snapshot-local"
                //)

                //rtMavenResolver (
                    //id: "MAVEN_RESOLVER",
                    //serverId: "jfrog-server",
                    //releaseRepo: "libs-release",
                    //snapshotRepo: "libs-snapshot"
                //) // Configure Artifactory Gradle resolver
            //}
        //}

        //stage ('Deploy Artifacts') {
            //steps {
                //rtMavenRun (
                    //tool: "maven", 
                    //pom: 'pom.xml',
                    //goals: 'clean install',
                    //deployerId: "MAVEN_DEPLOYER",
                    //resolverId: "MAVEN_RESOLVER"
                //)  // Deploy artifacts using Artifactory Gradle plugin
            //}
        //}
        
        //stage ('Publish build info') {
            //steps {
                //rtPublishBuildInfo (
                    //serverId: "jfrog-server"
                //)  // Publish build information to Artifactory
            //}
        //}
        
        //stage("Manage release") {
            //steps {
                //script {
                    //git url: 'https://github.com/mouhib19XX/Delivery-tool.git'  // Clone repository containing Python script
                    //sh "python script.py"  // Execute Python script
                //}
            //}
        //}
        
        stage('Build Docker Images') {
            steps {
                script {
                    // Loop through each microservice directory
                    for (service in microservices) {
                        // Navigate to the microservice directory
                        dir(service) {
                            // Build the Docker image using the Dockerfile in the directory
                            docker.build("${DOCKER_USER}/${service}:${IMAGE_TAG}")
                        }
                    }
                }
            }
        }
                
        stage("Trivy Images Scan") {
            steps {
                script {
                    // Loop through each microservice directory
                    for (service in microservices) {
                        sh "docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ${DOCKER_USER}/${service}:${IMAGE_TAG} --no-progress --exit-code 0 --severity HIGH,CRITICAL --format table --scanners vuln --timeout 50m > trivy_${service}.txt"  // Perform Trivy image scan 
                    }
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    // Loop through each microservice directory
                    for (service in microservices) {
                        // Push the Docker image to the container registry
                        docker.withRegistry('', DOCKER_PASS) {
                            docker.image("${DOCKER_USER}/${service}:${IMAGE_TAG}").push()
                        }
                    }
                }
            }
        }

        stage ('Cleanup Artifacts') {
            steps {
                script {
                    // Remove Docker images
                    for (service in microservices) {
                        sh "docker rmi ${DOCKER_USER}/${service}:${IMAGE_TAG}"  // Remove Docker images
                    }
                }
            }
        }
         stage('Update Kubernetes Deployment Files') {
            steps {
                script {
                    for (service in microservices) {
                        def deploymentFile = "k8s/${service}.yml"
                        sh "sed -i 's|image: ${DOCKER_USER}/${service}:.*|image: ${DOCKER_USER}/${service}:${IMAGE_TAG}|g' ${deploymentFile}"
                        sh "cat ${deploymentFile}"  //Display the updated file for verification
                    }
                }
            }
        }

        stage('Push Updated Deployment Files to Git') {
            steps {
                script {
                    // Add and commit the changes to the deployment files
                    sh """
                    git add k8s/
                    git commit -m 'Updated the deployment files with new image tags'
                    git push http://${user}:${pass}@github.com/mouhib19XX/ecommerce-microservices-.git main
                    """
                }
            }
        }
    }
}
