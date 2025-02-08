pipeline{
    agent any

    environment {
        CHROME_VERSION = '133.0.6943.60'
        CHROMEDRIVER_VERSION = '133.0.6943.60'
        CHROME_INSTALL_PATH = 'C:\\applications\\chrome.exe'
        CHROMEDRIVER_PATH = 'C:\\applications\\chromedriver.exe'
    }
    stages{
        stage('Checkout the code'){
            steps{
                git branch: 'main', url: 'https://github.com/Svetoslavss/devops-jenkins.git'
            }
        }
        stage('Setup .NET Core'){
            steps{
                bat '''
                    echo Intalling .NET SDK 6.0 
                    choco install dotnet-sdk --version=6.0.100 -y
                    '''
            }
        }
        stage('Uninstall Current Chrome'){
            steps{
                bat '''
                    echo Uninstalling current Chrome
                    choco uninstall googlechrome -y
                    '''
            }
        }
        stage('Install specific Chrome version'){
            steps{
                bat '''
                    echo Installing Chrome version %CHROME_VERSION%
                    choco install googlechrome --version=%CHROME_VERSION% -y --allow-downgrade --ignore-checksums
                    '''
            }
        }
        stage('Restore dependencies'){
            steps{
                bat '''
                    echo Restoring dependencies
                    dotnet restore
                    '''
            }
        }
        stage('Build'){
        steps{
            echo 'Building the project'
            bat 'dotnet build SEleniumIde.cln --configuration Release'
    }
        }
        stage('Test'){
            steps{
                echo 'Running tests'
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=testresults.trx"'
            }
        } 

        post{
            always{
                archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
                junit '/TestResults/*.trx'
            }
        }
}
}
