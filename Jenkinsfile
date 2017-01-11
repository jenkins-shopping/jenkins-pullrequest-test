def getCommitId() {
    sh returnStdout: true, 
       script: "git --no-pager show -s --format='%H'"
}

node("host-node"){
    def buildUser = wrap([$class: 'BuildUser']) { env.BUILD_USER }
    def commitId = getCommitId()
    try {
        stage("Prepare Environment") {
            stage = "Prepare Environment"
            git 'git@github.com:jenkins-shopping/jenkins-pullrequest-test.git'

            try {
                if (commitId != "") {
                    manager.addShortText(commitId, "black", "#FFFFE0", "1px", "grey")
                }
            } catch(err) {}

            checkout([
                $class: 'GitSCM',
                branches: [[name: '${sha1}']],
                doGenerateSubmoduleConfigurations: false,
                extensions: [[
                    $class: 'RelativeTargetDirectory',
                    relativeTargetDir: ""
                ]],
                submoduleCfg: [],
                userRemoteConfigs: [[
                    url: 'jenkins-shopping/jenkins-pullrequest-test'
                ]]
            ])
        }
  
    } catch (e) {}
}
