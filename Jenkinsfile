parallel_create_stages = [:]
parallel_destroy_stages = [:]

parallel_create_stages["Ubuntu 22"] = {
    stage("Create") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "cd ./roles/setup && molecule reset -s install-ubuntu2204 && molecule create -s install-ubuntu2204"
        }
    }
}

parallel_create_stages["Ubuntu 20"] = {
    stage("Create") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "cd ./roles/setup && molecule reset -s install-ubuntu2004 && molecule create -s install-ubuntu2004"
        }
    }
}

parallel_destroy_stages["Ubuntu 22"] = {
    stage("Destroy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "cd ./roles/setup && molecule destroy -s install-ubuntu2204"
        }
    }
}

parallel_destroy_stages["Ubuntu 20"] = {
    stage("Destroy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "cd ./roles/setup && molecule destroy -s install-ubuntu2004"
        }
    }
}


node {
    checkout scm

    withCredentials([usernamePassword(
            credentialsId: 'jenkins_infra_account',
            usernameVariable: 'VSPHERE_USER',
            passwordVariable: 'VSPHERE_PASSWORD'
    )]) {
        
        parallel(parallel_create_stages)
        
        parallel(parallel_destroy_stages)
    }
}