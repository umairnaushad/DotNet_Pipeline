def props
def gitRepo
def gitBranch
def deploymentType
def environment
def isAdminTool, isInterlayer, isWeb, isDatabase 
def versionNo, buildNo
def tagName
def strArray

pipeline {

    options {
        buildDiscarder logRotator(numToKeepStr: '3')
    }

    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    props = readProperties file:'system.properties'
                    gitRepo = props.gitRepo
                    gitBranch = props.gitBranch                    
                    tagName = props.tagName.toLowerCase()
                    echo 'gitRepo: ' + gitRepo + ', gitBranch: ' + gitBranch + ', tagName: ' + tagName
                    strArray = tagName.split('-')
                    deploymentType = strArray[0]
                    environment = strArray[1]
                    versionNo = strArray[strArray.size() - 2]
                    buildNo = strArray[strArray.size() - 1]
                    echo 'deploymentType: ' + deploymentType + ', environment: ' + environment + ', versionNo: ' + versionNo + ', buildNo: ' + buildNo
                    echo 'strArray size: ' + strArray.size()
                    echo 'Expression: ' + tagName.contains('interlayer')

                    git branch: gitBranch, credentialsId: 'github_Umair', url: gitRepo 
                }                
            }
        }

        stage ('Build Interlayer') {
            when {
                expression { tagName.contains('interlayer') }
            }
            steps {
                script {                    
                    echo "Building interlayer"
                    echo "\"${tool 'v15.0'}\\msbuild.exe\""
                    bat "\"${tool 'v15.0'}\\msbuild.exe\" \"${WORKSPACE}\\Source\\VS Solution\\Desktop Application.sln\" /t:Build /p:Configuration=Release /p:TargetFramework=v4.7.2 /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"
                }
            }
        }

        stage ('Build AdminTool') {
            when {
                expression { tagName.contains('admintool') }
            }
            steps {
                echo "Building AdminTool"
            }
        }

        stage ('Build WebApplication') {
            when {
                expression { tagName.contains('webapplication') }
            }
            steps {
                echo "Building WebApplication"
            }

        }

        

        stage ('Upload Artifacts') {
            steps {
                script {
                    fileOperations([fileZipOperation('./Exe')])
                    nexusArtifactUploader artifacts: [[artifactId: 'executables', classifier: '', file: 'Exe.zip', type: 'zip']], credentialsId: 'NEXUS', groupId: 'dotnet_app', nexusUrl: 'localhost:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'test_repo/Nexus', version: '1.$BUILD_NUMBER'

                }
            }
        }

    }
}