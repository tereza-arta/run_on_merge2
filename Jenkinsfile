def gv

pipeline{
    agent any
    environment { 
        SOURCE_BRANCH = sh(
                    script: "git log --merges --first-parent origin/main | awk '/Merge pull request/{split(\$NF, arr, \"/\"); print arr[2]}' | head -1",
                    returnStdout: true
                ).trim()
        TARGET_BRANCH = sh(
                    script: "git log --merges --first-parent origin/main | tail -n 1",
                    returnStdout: true
                ).trim() 
        SHOULD_RUN = 'true'
    }
    stages {
        stage('Init') {
            steps {
                script {
                    if (env.SHOULD_RUN == 'true') {
                        gv = load "script.groovy"
                    }
                }
            }
        }
        stage('Check') {
            when {
                    expression { SOURCE_BRANCH == 'other_branch' }
            }
            steps {
                echo "Merging branch is <other_branch>"
                script {
                    gv.function()
                    echo "Receiving branch is <dev>"
                }
            }
        }
    }
}
