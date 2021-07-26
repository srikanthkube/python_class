pipeline {
    agent any

    environment {
        // server = Artifactory.server('nikethdock.jfrog.io')
        // rtDocker = Artifactory.docker server: server
        // buildInfo = Artifactory.newBuildInfo()
        ARTIFACTORY_DOCKER_REGISTRY = "nikethdock.jfrog.io/docker-jenkins-docker-local"
    }

    stages {

        stage ('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/srikanthkube/python_class.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "https://nikethdock.jfrog.io/artifactory",
                    credentialsId: 'artifactory'
                )
            }
        }

        stage('Config Build Info') {
            steps {
                rtBuildInfo (
                    captureEnv: true
                )
            }
        }

        stage ('Build docker image') {
            steps {
                script {
                    def dockerfile = 'Dockerfile'
                    docker.build(ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:${env.BUILD_ID}", ".")
                }
            }
        }

        stage ('Push image to Artifactory') {
            steps {
                // buildInfo = rtDocker.push ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:${env.BUILD_ID}", "docker-jenkins-docker-local"
                rtDockerPush (
                    serverId: "ARTIFACTORY_SERVER",
                    image: ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:${env.BUILD_ID}",
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    //host: 'tcp://127.0.0.1:1234',
                    targetRepo: 'docker-jenkins-docker-local', // where to copy to (from docker-virtual)
                    // Attach custom properties to the published artifacts:
                    properties: 'project-name=docker1;status=stable'
                )
            }
        }

        // stage ('Publish build info') {
        //     steps {
        //         rtPublishBuildInfo (
        //         serverId: "ARTIFACTORY_SERVER"
        //         )
        //     }
        // }
    }
}
