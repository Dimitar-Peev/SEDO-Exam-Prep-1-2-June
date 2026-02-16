pipeline {
    agent any

    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Restore Dependencies") {
            steps {
                echo 'Restoring NuGet packages...'
                bat 'dotnet restore'
            }
        }

        stage("Build") {
            steps {
                echo 'Building the application...'
                bat 'dotnet build --no-restore'
            }
        }

        stage("Test"){
            when {
                anyOf {
                    branch 'main'
                    branch 'feature/*'
                }
            }
            
            steps {
                echo 'Running Unit and Integration tests...'
                bat 'dotnet test --configuration Release --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished.'
        }
        success {
            echo 'Build and Tests passed successfully!'
        }
        failure {
            echo 'Build or Tests failed. Please check the console output.'
        }
    }
}
