# Jenkins Project Diagram Overview
![1594373757598](https://github.com/mostafayounis1/Nti-Jenkins-Lab/assets/167571650/467cd743-8373-443b-a69c-29829863fd20)


## Shared Libirary Repository
https://github.com/mostafayounis1/Jenkins-Shared-Library.git
## Pipeline Overview

The Jenkins pipeline follows these stages to build, push, edite and deploy Docker images to dockerhub and an Minikube cluster:

1. **Build Docker Image:** Build docker image.

2. **Push Docker Image To DockerHub:** Push docker image to docker hub.

3. **Edit new image in deployment.yaml file:** Edit new image in deployment.yaml file.

4. **Deploy to k8s:**  Deploy image to k8s minikube cluster.


## Pipeline Steps

### Build Docker Image:

```
 stage('Build Docker image from Dockerfile in GitHub') {
            steps {
                script {
                 	
                 		buildDockerImage("${imageName}")
                      
                }
            }
        }
```



### Push Docker Image To DockerHub:

```
stage('Push image to Docker hub') {
            steps {
                script {
                 	
                 		pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                      
                }
            }
        }

```

### Edit new image in deployment.yaml file:

```
stage('Edit new image in deployment.yaml file') {
            steps {
                script { 
                	dir('k8s') {
				        editNewImage("${imageName}")
                    	}
                }
            }
        }
```
### Deploy to k8s:

```
stage('Deploy on k8s Cluster') {
            steps {
                script { 
                	dir('k8s') {
				         deployOnKubernetes("${k8sCredentialsID}")
                    }
                }
            }
        }

```


### Post-Build Actions
In case of pipeline success or failure, the following messages will be displayed:
```
 post {
        always {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline always succeeded"
        }
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
```
----
### K8s caredntials
![Screenshot 2024-05-01 225903](https://github.com/mostafayounis1/Nti-Jenkins-Lab/assets/167571650/1b459746-4684-4f35-be91-8cf084bc6a05)

### Successfully Run  Pipeline

![Screenshot 2024-05-04 232200](https://github.com/mostafayounis1/Nti-Jenkins-Lab/assets/167571650/04a23ecd-5577-4b23-819c-592502b16d31)



### Successfully Run Test  Pipeline On k8s minikube cluster
![Screenshot 2024-05-04 232407](https://github.com/mostafayounis1/Nti-Jenkins-Lab/assets/167571650/db72adcc-98d8-4043-b199-faef8629cfe6)



