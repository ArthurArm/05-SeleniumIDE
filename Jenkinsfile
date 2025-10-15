pipeline {
    agent any
    stages {
        stage('Install Chrome and ChromeDriver') {
            steps {
                sh '''
                sudo apt-get update -y
                sudo apt-get install -y wget unzip curl
                wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
                sudo apt-get install -y ./google-chrome-stable_current_amd64.deb
                VER=$(google-chrome --version | grep -oE "[0-9]+" | head -1)
                wget -q -O /tmp/chromedriver.zip https://chromedriver.storage.googleapis.com/${VER}/chromedriver_linux64.zip
                sudo unzip -q /tmp/chromedriver.zip -d /usr/local/bin/
                sudo chmod +x /usr/local/bin/chromedriver
                '''
            }
        }
        stage("Restore project dependencies") {
            steps{
                sh 'dotnet restore'
            }
        }
        stage("Build the project") {
            steps {
                sh 'dotnet build --no-restore'
            }
        }
        stage("Run tests") {
            steps  {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}
