node {
    def commit_id
    stage('Prepartion') {
        checkout scm
        sh "git rev-parse --short HEAD > .git/commit-id"
        commit_id = readFile('.git/commit-id').trim()
    }
    stage('Docker build and push') {
        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
            def app = docker.build("nikethdock/python-jenkins-docker:${commit_id}", '.').push()
        }
    }
}
