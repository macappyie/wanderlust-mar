pipeline {
    agent any
    environment{
        SONAR_HOME= tool "sonar"
    }
    stages {
        stage("clone code from github"){
            steps{
                git url:"https://github.com/macappyie/wanderlust-mar.git", branch: "devsecops"
            }
        }
        stage("sonarqube quality analysis"){
            steps{
                withSonarQubeEnv("sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust-mar -Dsonar.projectKey=wanderlust-mar"
                }
            }
        }
        stage("OWASP dependency check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ --noupdate', odcInstallation: 'dc'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("sonar quality gate scan"){
            steps{
                timeout(time: 2, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy file system scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("deploy usig docker compose"){
            steps{
                sh """
                docker-compose down --remove-orphans
                docker-compose up -d --build
                docker image prune -f
                """
            }
        }
    }
}

