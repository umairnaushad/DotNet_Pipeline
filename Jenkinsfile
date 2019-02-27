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
        stage('Initialization') {
            steps {
                script {
                    props = readProperties file:'system.properties'
                    gitRepo = props.gitRepo
                    gitBranch = props.gitBranch                    
                    tagName = props.tagName
                    echo 'gitRepo: ' + gitRepo + ', gitBranch: ' + gitBranch + ', tagName: ' + tagName
                    strArray = tagName.toLowerCase().split('-')
                    deploymentType = strArray[0]
                    environment = strArray[1]
                    versionNo = strArray[strArray.size() - 2]
                    buildNo = strArray[strArray.size() - 1]
                    echo 'deploymentType: ' + deploymentType + ', environment: ' + environment + ', versionNo: ' + versionNo + ', buildNo: ' + buildNo
                    echo 'strArray size: ' + strArray.size()
                }                
            }
        }

        stage ('Build Interlayer') {
            when {
                expression { tagName.contains('interlayer') == true }
            }
            steps {
                echo "Building interlayer"
            }
        }

        stage ('Build AdminTool') {
            when {
                expression { tagName.contains('admintool') == true }
            }
            steps {
                echo "Building AdminTool"
            }
        }

        stage ('Build WebApplication') {
            when {
                expression { tagName.contains('webapplication') == true }
            }
            steps {
                echo "Building WebApplication"
            }
        }
    }
}