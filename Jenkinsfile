pipeline {
    agent { label 'Node-Linux' }

    environment {
        GIT_REPO = 'https://github.com/bharathsavadatti447/git_assignment_25092025.git'
        BRANCH   = 'master'
    }

    stages {
        stage('Clone') {
            steps {
                echo "Cloning the repo from Github ........."
                git branch: "${BRANCH}",
                    url: "${GIT_REPO}",
                    credentialsId: 'a5e5c631-8e24-48ee-9844-ea7c8b7a658d'
            }
        }

        stage('Build') {
            steps {
                sh 'dos2unix build.sh'
                sh 'chmod +x build.sh'
                sh './build.sh'
                archiveArtifacts artifacts: 'build/myfirmware.*', fingerprint: true
            }
        }

        stage('Lint') {
            steps {
                echo "Running lint checks..."
                // Add your lint commands here, e.g.,
                // sh 'lint-tool build/'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success/failure/unstable).'
        }
        unstable {
            echo 'Build marked as UNSTABLE!'
            emailext(
                subject: "Build Unstable: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
                body: """<p>Build became <b>UNSTABLE</b> in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
        failure {
            echo 'Build failed!'
            emailext(
                subject: "Build Failed: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
                body: """<p>Build failed in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}
