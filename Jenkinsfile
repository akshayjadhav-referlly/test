pipeline {
    agent any
    stages {

        stage('Checkout') {
            steps {
                checkout scm        // ← This clones your repo into the workspace
            }
        }

        stage('Detect Changes') {
            steps {
                script {
                    def changedFiles = sh(
                        script: "git diff --name-only HEAD~1 HEAD 2>/dev/null || git show --name-only --pretty=format: HEAD",
                        returnStdout: true
                    ).trim()
                    echo "Changed files:\n${changedFiles}"
                    env.BUILD_FRONTEND  = changedFiles.contains('frontend/')      ? 'true' : 'false'
                    env.BUILD_BACKEND   = changedFiles.contains('backend/')       ? 'true' : 'false'
                    env.BUILD_MIDDLEEND = changedFiles.contains('middleend-next/') ? 'true' : 'false'
                }
            }
        }

        stage('Frontend') {
            when { expression { env.BUILD_FRONTEND == 'true' } }
            steps {
                echo "Frontend folder changed — triggering Frontend stage!"
            }
        }

        stage('Backend') {
            when { expression { env.BUILD_BACKEND == 'true' } }
            steps {
                echo "Backend folder changed — triggering Backend stage!"
            }
        }

        stage('Middleend') {
            when { expression { env.BUILD_MIDDLEEND == 'true' } }
            steps {
                echo "Middleend folder changed — triggering Middleend stage!"
            }
        }
    }
}
