node {

    stage 'Checkout'
    checkout scm

    stage 'Build application war file'
    // Build petclinic in a Maven3+JDK8 Docker container
    docker.image('maven:3-jdk-8').inside('-v /.m2:/root/.m2') {
        sh 'mvn -B package -DskipTests'
    }

    stage 'Build application Docker image'
    def appImg = docker.build("eicc-dev-1470663142829/petclinic")

    stage 'Push to GCR'
    docker.withRegistry('https://gcr.io', 'gcr:eicc-dev-1470663142829') {
        appImg.push();
    }

    // stage 'Run app on Kubernetes'
    // withKubernetes( serverUrl: 'https://146.148.36.159', credentialsId: 'kubeadmin' ) {
    //      sh 'kubectl run petclinic --image=gcr.io/nicolas-deloof/petclinic --port=8080'
    //}

}
