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

                // println sh(script: "cat testPR/file2", returnStdout: true)

                properties properties: [pipelineTriggers([]), [$class: 'GithubProjectProperty', displayName: 'Jenkins']]

                step([$class: 'GitHubCommitStatusSetter',
                    statusResultSource: [
                        $class: 'ConditionalStatusResultSource',
                        results: [[
                            $class: 'BetterThanOrEqualBuildResult',
                            message: 'Build success',
                            result: 'SUCCESS',
                            state: 'SUCCESS'
                            ]]
                        ]
                    ]
                )
                step([$class: 'GitHubCommitStatusSetter',
                  contextSource: [$class: 'ManuallyEnteredCommitContextSource', context: 'Test Context'],
                  statusResultSource: [$class: 'ConditionalStatusResultSource',
                                 results: [[$class: 'AnyBuildResult',
                                           message: 'test message',
                                           state: 'SUCCESS']]]])

                def commitId = sh(script: bash("git --no-pager show -s --format='%H'"),
                      returnStdout: true).trim()

                if (commitId) {
                    manager.addShortText(commitId, "black", "#FFFFE0", "1px", "grey")
                }
            }
        }
    } catch (e) {throw e}
}
