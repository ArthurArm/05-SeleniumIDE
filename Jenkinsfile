pipeline {
    agent any
    stages {
        stage('Install Chrome and ChromeDriver') {
            steps {
                sh '''
                    apt-get update -y
                    apt-get install -y sudo
                    
                    sudo apt-get update -y
                    sudo apt-get install -y wget unzip google-chrome-stable
                    
                    # Install ChromeDriver
                    sudo wget -O /tmp/chromedriver.zip https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/120.0.6099.109/linux64/chromedriver-linux64.zip
                    sudo unzip -o /tmp/chromedriver.zip -d /tmp/
                    sudo mv /tmp/chromedriver-linux64/chromedriver /usr/local/bin/
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
