@NonCPS
def bash(script) {
    "#!/bin/bash\n${script}"
}


@NonCPS
def printParams() {
  env.getEnvironment().each { name, value -> println "Name: $name -> Value $value" }
}


node("host-node"){
    def buildUser = wrap([$class: 'BuildUser']) { env.BUILD_USER }
    try {
        def commitId
        stage("Prepare Environment") {

            printParams()  
            dir('checkout_folder'){
              checkout scm
            }
            println sh(script: 'pwd', returnStdout: true)
            println sh(script: 'ls -la', returnStdout: true)
            def ret = sh(script: 'cat checkout_folder/testPR/file2', returnStdout: true)
            println ret

            commitId = sh(script: bash("git --no-pager show -s --format='%H'"),
                              returnStdout: true).trim()  

            if (commitId) {
                manager.addShortText(env.ghprbPullDescription + "\nsha1: " + env.sha1, "black", "#FFFFE0", "1px", "grey")
            }

        }
  
    } catch (e) {}
}
