node {
    def application = "pythonapp"
    def dockerhubaccountid = "erisjat"
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([ credentialsId: "jenkins-hubdocker", url: "" ]) {
        app.push()
        app.push("latest")
    }
    }

    stage('Deploy') {
        sh ("docker run -p 3333:3333 -d ${dockerhubaccountid}/${application}:${BUILD_NUMBER} /bin/bash -c "echo 'Hello World'; tail -f /dev/null"")
    }

    stage('Remove old images') {
        // remove old docker images
        sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}