pipeline {
    agent any

    environment {
        server = Artifactory.server('nikethdock.jfrog.io')
        rtDocker = Artifactory.docker server: server
        buildInfo = Artifactory.newBuildInfo()
        ARTIFACTORY_DOCKER_REGISTRY='nikethdock.jfrog.io/docker-jenkins-docker-local'
    }

    stages {

        stage('Config Build Info') {
            steps {
                buildInfo.env.capture = true
            }
        }

        stage ('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/srikanthkube/python_class.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                id: 'ARTIFACTORY_SERVER',
                url: "https://nikethdock.jfrog.io/artifactory",
                credentialsId: 'artifactory'
                )
            }
        }

        stage ('Build docker image') {
            steps {
                docker.build(ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:${env.BUILD_ID}", ".")
            }
        }

        stage ('Push image to Artifactory') {
            steps {
                buildInfo = rtDocker.push ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:${env.BUILD_ID}", "docker-jenkins-docker-local"
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                serverId: 'ARTIFACTORY_SERVER'
                )
            }
        }
    }
}
