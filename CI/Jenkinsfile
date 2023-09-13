pipeline {
    agent any
    // tools{
    //     maven 'dinesh-maven'
    // }

    environment {
                AWS_ACCESS_KEY_ID = credentials('jenkins-aws-access-key-id')
                AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
                SONAR_TOKEN = credentials('sonar-creds')
                DOCKERHUB_USERNAME = "shivanishivani"
                APP_NAME = "clinic09"
                IMAGE_TAG = "${BUILD_NUMBER}"
                IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
                //REGISTRY_CREDS = 'docker-creds'
                ECRURL = "https://953423807780.dkr.ecr.us-east-2.amazonaws.com/pc-ecr"
                ECR_REGISTRY = "953423807780.dkr.ecr.us-east-2.amazonaws.com/pc-ecr"
                //ECR_IMAGE_NAME = "pc-ecr"
            }
     parameters {
     string(name: 'ECRURL', defaultValue: 'https://953423807780.dkr.ecr.us-east-2.amazonaws.com', description: 'Please Enter your Docker ECR REGISTRY URL?')
     string(name: 'REPO', defaultValue: 'pc-ecr', description: 'Please Enter your Docker Repo Name?')
     string(name: 'REGION', defaultValue: 'us-east-2', description: 'Please Enter your AWS Region?')
    }


    stages {

        
        stage ('Sonarcloud scan') {
            steps {  
                echo 'scanning code' 
                script {
                    dir ('application-code-g') {
                    env.SONAR_TOKEN = "${SONAR_TOKEN}"
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=august_project_job_ci'
                    }
                }
            }  
        }
        
        stage ('Build jar') {
            steps {  
                echo 'Building jar' 
                script {
                    dir ('application-code-g') {
                        sh 'mvn clean install -DskipTests,spring.profiles.active=mysql'
                    }
                }
            }  
        }
        
        stage('Stage Artifacts to Jfrog') {
                steps {
                    script {
                        def server = Artifactory.server 'my-jfrog2' 
                        def uploadSpec = """{    
                            "files": [{
                            "pattern": "/var/lib/jenkins/workspace/ci_pipeline/application-code-g/target/spring-petclinic-3.1.0-SNAPSHOT.jar", 
                            "target": "my-jfrog2",
                            "recursive": "false"        
                            }]
                        }"""   
                        server.upload(uploadSpec)       
                    }
            }
        } 
        stage ('Docker Image Build') {  
            steps {
                    script {   
                        dir ('application-code-g') {
                            dockerTag = params.REPO + ":" + "${IMAGE_TAG}"
                            docker.withRegistry( params.ECRURL, 'ecr:us-east-2:aws-creds' ) {
                            myImage = docker.build(dockerTag)
                        }
                    }
                }
            }  
        }
        
        stage ('Docker scan with TRIVY') {  
            steps {
                    script {   
                        dir ('application-code-g') {
                            sh ("docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $PWD:/tmp/.cache/ aquasec/trivy:0.10.0 ${params.REPO}:${IMAGE_TAG}")
                    }
                }
            }
        }
        stage('Explore Image Layers with DIVE') { 
            steps { 
                script {
                    env.dockerTag = params.REPO + ":" + "${IMAGE_TAG}"
                    sh "docker run --rm -i \
                        -v /var/run/docker.sock:/var/run/docker.sock \
                        wagoodman/dive:latest --ci ${dockerTag} --lowestEfficiency=0.8 --highestUserWastedPercent=0.45"
                }                        
            }            
        }
        stage ('Docker image Push to ECR') {  
            steps {
                    script {   
                        dir ('application-code-g') {
                            dockerTag = params.REPO + ":" + "${IMAGE_TAG}"
                            docker.withRegistry( params.ECRURL, 'ecr:us-east-2:aws-creds' ) {
                            myImage.push()
                        }
                    }
                }
            }  
        }    
        stage ('Clean docker images') {
            steps {  
                echo 'Cleaning the docker images' 
                script {
                        dir ('application-code-g') {
                        sh ("docker rmi ${params.REPO}:${IMAGE_TAG}")
                        sh ("docker rmi ${ECR_REGISTRY}:${IMAGE_TAG}")
                    }
                }
            } 
        }
        stage ('Trigger cd pipeline') {
            steps {  
                echo 'Triggering cd pipeline' 
                script {
                        sh """
                            curl http://3.128.173.147:8080/job/jmd_cd_pipeline/buildWithParameters?token=gitops-config \
                                --user dinesh-jenkins:118a7f9f0161191a872a6d7059d6a6e370 \
                                --data IMAGE_TAG=${IMAGE_TAG} --data verbosity=high \
                                -H content-type:application/x-www-form-urlencoded \
                                -H cache-control:no-cache
                            """
                    }
                }
        } 
   
    } 
}

