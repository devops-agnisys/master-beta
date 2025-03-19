pipeline {
    agent any

    environment {
        PRIMARY_REPO = 'https://github.com/devops-agnisys/master-beta.git'
        SECONDARY_REPO = 'https://github.com/devops-agnisys/java.git'
    }

    stages {
        stage('Clone Repositories') {
            steps {
                // Clean workspace
                deleteDir()

                // Clone primary repo (current one)
                git url: "${PRIMARY_REPO}", branch: 'main'

                // Clone secondary repo
                dir('secondary') {
                    git url: "${SECONDARY_REPO}", branch: 'main'
                }

                echo "Repos cloned:"
                sh 'ls -l'
                sh 'ls -l secondary'
            }
        }

        stage('Build Artifacts') {
            steps {
                // Sample Java compilation (adjust for Maven/Gradle if needed)
                sh '''
                    mkdir -p build
                    # Compile primary repo Java code
                    find src -name "*.java" > sources.txt
                    # Compile secondary repo Java code
                    find secondary/src -name "*.java" >> sources.txt

                    javac -d build @sources.txt
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**/*', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build successful and artifacts archived!"
        }
        failure {
            echo "❌ Build failed. Check logs."
        }
    }
}
