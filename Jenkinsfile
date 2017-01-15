@NonCPS
def bash(script) {
    "#!/bin/bash\n${script}"
}

node("host-node"){
    def buildUser = wrap([$class: 'BuildUser']) { env.BUILD_USER }
    try {
        def commitId
        stage("Prepare Environment") {

            println 'ChangeID: ' + env.CHANGE_ID
            println 'BRANCH_NAME: ' + env.BRANCH_NAME     
            checkout([
                $class: 'GitSCM',
                branches: [[name: env.BRANCH_NAME]],
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
            println sh(script: 'pwd', returnStdout: true)
            println sh(script: 'ls -la', returnStdout: true)
            def ret = sh(script: 'cat testPR/file2', returnStdout: true)
            println ret

            commitId = sh(script: bash("git --no-pager show -s --format='%H'"),
                              returnStdout: true).trim()  
                                          
            if (commitId) {
                manager.addShortText(env.ghprbPullDescription + "\nsha1: " + env.sha1, "black", "#FFFFE0", "1px", "grey")
            }

        }
  
    } catch (e) {}
}
