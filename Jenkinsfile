parallel_stages = [:]

parallel_stages["Ubuntu 22"] = {
    stage("Ubuntu 22 - Create") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule create -s install-ubuntu2204"
        }
    }

    stage("Ubuntu 22 - Install") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s install-ubuntu2204  --destroy never"
        }
    }

    stage("Ubuntu 22 - ScaleUp") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s scaleup-ubuntu2204  --destroy never"
        }
    }

    stage("Ubuntu 22 - ScaleDown") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s scaledown-ubuntu2204  --destroy never"
        }
    }

    stage("Ubuntu 22 - Deploy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/use && molecule test -s deploy-ubuntu2204  --destroy never"
        }
    }

    stage("Ubuntu 22 - Undeploy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/use && molecule test -s undeploy-ubuntu2204  --destroy never"
        }
    }

    stage("Ubuntu 22 - Uninstall") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s uninstall-ubuntu2204  --destroy never"
        }
    }

    stage("Ubuntu 22 - Destroy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml"
            sh "cd ./roles/setup && molecule destroy -s install-ubuntu2204"
        }
    }
}

parallel_stages["Ubuntu 20"] = {
    stage("Ubuntu 20 - Create") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml"
            sh "cd ./roles/setup && molecule create -s install-ubuntu2004"
        }
    }

    stage("Ubuntu 20 - Install") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s install-ubuntu2004  --destroy never"
        }
    }

    stage("Ubuntu 20 - ScaleUp") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s scaleup-ubuntu2004  --destroy never"
        }
    }

    stage("Ubuntu 20 - ScaleDown") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s scaledown-ubuntu2004  --destroy never"
        }
    }

    stage("Ubuntu 20 - Deploy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/use && molecule test -s deploy-ubuntu2004  --destroy never"
        }
    }

    stage("Ubuntu 20 - Undeploy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/use && molecule test -s undeploy-ubuntu2004  --destroy never"
        }
    }

    stage("Ubuntu 20 - Uninstall") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml && cd ./roles/setup && molecule test -s uninstall-ubuntu2004  --destroy never"
        }
    }

    stage("Ubuntu 20 - Destroy") {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {
            sh "ansible-galaxy install -f -r requirements.yml"
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
        
        parallel(parallel_stages)
    }
}