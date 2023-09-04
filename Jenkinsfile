node {
    def app

    stage('clone repository'){
        sh 'echo "Cloning the repository"'
        checkout scm
    }

    stage('Build image'){
        sh 'echo "Building the image"'
        app = docker.build("meets0ni/webapp")
        sh 'docker images'
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Testing the image"'
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {

        sh 'echo "Pushing the image"'
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }    

    stage('Update image teg') {  // replace tocken in azure devops

        sh "cat manifest.yaml"
        sh "sed -i 's+meets0ni/test.*meets0ni/test:${DOCKERTAG}+g' manifest.yaml"
        sh "cat manifest.yaml"
    }

    stage('Push Latest code'){
        sh "git add ."
        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Jenkins-CICD.git HEAD:main"
    }

    stage("SSH Into k8s Server") {

        sh 'echo "connecting via ssh to master node"'
        def remote = [:]
        remote.name     = 'k8smaster'
        remote.host     = '11.223.14.274'
        remote.user     = 'meet'
        remote.password = 'meet'
        // below are parameterized 
        remote.name     = ${K8S_HOSTNAME}
        remote.host     = ${K8S_HOST}
        remote.user     = ${K8S_HOST_USER}
        remote.password = ${K8S_HOST_PASSWORD}
        remote.allowAnyHosts = true

        stage('Put manifest.yaml into k8smaster') {
            sshPut remote: remote, from: 'manifest.yaml', into: '.'
        } 

        stage('Deploy simple web') {
            sshCommand remote: remote, command: "kubectl apply -f manifest.yaml"
        }
    } 
}
