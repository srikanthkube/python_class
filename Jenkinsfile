pipeline {
    agent {
        label 'docker-artifactory-test'
    }

    def server = Artifactory.server('nikethdock.jfrog.io')
    def rtDocker = Artifactory.docker server: server
    def buildInfo = Artifactory.newBuildInfo()
    def ARTIFACTORY_DOCKER_REGISTRY='nikethdock.jfrog.io/docker-jenkins-docker-local'
    buildInfo.env.capture = true

    stages {
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
