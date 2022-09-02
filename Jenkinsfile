def scenarios = [
    "ubuntu1804" : [
            ["setup",   "install"],
            ["setup",   "scaleup"],
            ["setup",   "scaledown"],
            ["use",     "deploy"],
            ["use",     "undeploy"],
            ["setup",   "uninstall"],
    ],
    "ubuntu2004": [
            ["setup",   "install"],
            ["setup",   "scaleup"],
            ["setup",   "scaledown"],
            ["use",     "deploy"],
            ["use",     "undeploy"],
            ["setup",   "uninstall"],
    ],
    "ubuntu2204" : [
            ["setup",   "install"],
            ["setup",   "scaleup"],
            ["setup",   "scaledown"],
            ["use",     "deploy"],
            ["use",     "undeploy"],
            ["setup",   "uninstall"],
    ],
    "centos7" : [
            ["setup",   "install"],
            ["setup",   "scaleup"],
            ["setup",   "scaledown"],
            ["use",     "deploy"],
            ["use",     "undeploy"],
            ["setup",   "uninstall"],
    ],
    "centos8" : [
            ["setup",   "install"],
            ["setup",   "scaleup"],
            ["setup",   "scaledown"],
            ["use",     "deploy"],
            ["use",     "undeploy"],
            ["setup",   "uninstall"],
    ]
]

parallel_stages = [:]

for (kv in mapToList(scenarios)) {
    def platform = kv[0]
    def testList = kv[1]

    parallel_stages[platform] = {
        docker.image('fabos4ai/molecule:4.0.1').inside('-u root') {

            stage("${platform} - Dependencies") {
                sh "ansible-galaxy install -f -r requirements.yml"
            }

            for (int i = 0; i < testList.size(); i++) {
                def role = testList[i][0]
                def scenario = testList[i][1]

                stage("${platform} - ${scenario}") {
                    sh "cd ./roles/${role} && molecule test -s ${scenario}-${platform}  --destroy never"
                }
            }
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

        stage("dependencies") {
            sh "ansible-galaxy install -f -r requirements.yml"
        }

        try {
            for (kv in mapToList(scenarios)) {
                def platform = kv[0]
                def testList = kv[1]

                stage("${platform} - Create") {
                    sh " cd ./roles/setup && molecule reset -s install-${platform} && molecule create -s install-${platform}"
                }
            }

            parallel(parallel_stages)
        } finally {
            for (kv in mapToList(scenarios)) {
                def platform = kv[0]
                def testList = kv[1]

                stage("${platform} - Destroy") {
                    sh "cd ./roles/setup && molecule destroy -s install-${platform}"
                }
            }
        }
    }
}

@NonCPS
List<List<?>> mapToList(Map map) {
    return map.collect { it ->
        [it.key, it.value]
    }
}