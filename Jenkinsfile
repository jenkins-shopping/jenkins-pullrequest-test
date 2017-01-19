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

        def workDir = 'checkout_folder'

        stage("Prepare Environment") {

            // printParams()  
            dir(workDir){
                checkout scm

                println sh(script: "cat testPR/file2", returnStdout: true)

                def commitId = sh(script: bash("git --no-pager show -s --format='%H'"),
                      returnStdout: true).trim()  

                if (commitId) {
                    manager.addShortText(commitId, "black", "#FFFFE0", "1px", "grey")
                }              
            }


        }
  
    } catch (e) {}
}
