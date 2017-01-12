@NonCPS
def bash(script) {
    "#!/bin/bash\n${script}"
}

node("host-node"){
    def buildUser = wrap([$class: 'BuildUser']) { env.BUILD_USER }
    try {
        def commitId
        stage("Prepare Environment") {
            stage = "Prepare Environment"
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
                    url: 'git@github.com:jenkins-shopping/jenkins-pullrequest-test.git',
                    refspec: '+refs/pull/*:refs/remotes/origin/pr/*'
                ]]
            ])
            commitId = sh(script: bash("git --no-pager show -s --format='%H'"),
                              returnStdout: true).trim()
            if (commitId) {
                manager.addShortText(env.ghprbPullDescription + "\nsha1: " + env.sha1, "black", "#FFFFE0", "1px", "grey")
            }

        }
  
    } catch (e) {}
}
