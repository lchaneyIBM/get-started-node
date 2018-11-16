node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${MYNAMESPACE}/myapp")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            //app.push("${env.BUILD_NUMBER}")
            //app.push("latest")
            app.push("v1.0.0")
        }
    }
    stage('Deploy') {
        sh """
       cat "kubernetes/deployment.yaml" | sed "s/<REGISTRY>/${MYREGISTRY}/g" | sed "s/<NAMESPACE>/${MYNAMESPACE}/g" | kubectl apply -n default -f  -
            """
    }
}
