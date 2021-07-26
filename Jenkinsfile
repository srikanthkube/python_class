node {
    def server = Artifactory.server('nikethdock.jfrog.io')
    def buildInfo = Artifactory.newBuildInfo()
    def ARTIFACTORY_DOCKER_REGISTRY='nikethdock.jfrog.io/docker-jenkins-docker-local'
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
                    url: "https://nikethdock.jfrog.io",
                    credentialsId: artifactory
                )
            }
        }
        stage ('Build docker image') {
            steps {
                script {
                    docker.build(ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:latest:${env.BUILD_ID}", ".")
                }
            }
        }
        stage ('Push image to Artifactory') {
            buildInfo = rtDocker.push ARTIFACTORY_DOCKER_REGISTRY + "/hello-dock-jen:${env.BUILD_ID}", "docker-jenkins-docker-local"
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}
