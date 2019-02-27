def props
def gitRepo
def gitBranch
def deploymentType
def environment
def isAdminTool, isInterlayer, isWeb, isDatabase 
def tagName
def strArray

pipeline {

    options {
        buildDiscarder logRotator(numToKeepStr: '3')
    }

    agent any

    stages {
        stage("Pipeline Info") {
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
                    echo 'deploymentType: ' + deploymentType + ', environment: ' + environment
                    echo 'strArray size: ' + strArray.size()
                }                
            }
        }
    }
}