pipeline {
    agent any

    environment {
        PRIMARY_REPO = 'https://github.com/devops-agnisys/master-beta.git'
        SECONDARY_REPO = 'https://github.com/devops-agnisys/java.git'
    }

    stages {
        stage('Checkout Primary Repo') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: "${env.PRIMARY_REPO}"]]
                ])
            }
        }

        stage('Clone Secondary Repo') {
            steps {
                dir('secondary') {
                    git branch: 'main', url: "${env.SECONDARY_REPO}"
                }
            }
        }

        stage('Build Artifacts') {
            steps {
                sh '''
                    mkdir -p build

                    # Gather all .java files
                    find src -name '*.java' > sources.txt
                    find secondary/src -name '*.java' >> sources.txt

                    # Compile using javac
                    javac -d build @sources.txt
                '''
            }
        }

        stage('Archive Artifacts') {
            when {
                expression { fileExists('build') }
            }
            steps {
                archiveArtifacts artifacts: 'build/**/*.class', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully.'
        }
        failure {
            echo '❌ Build failed. Check logs.'
        }
        always {
            cleanWs()
        }
    }
}
