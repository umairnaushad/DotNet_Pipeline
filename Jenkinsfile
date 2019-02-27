def props
def gitRepo
def gitBranch
def deploymentType
def environment
def isAdminTool, isInterlayer, isWeb, isDatabase 
def tagName


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
                    gitRepo=props.gitRepo
                    gitBranch=props.gitBranch                    
                    tagName = props.tagName
                    echo 'gitRepo: ' + gitRepo
                    echo 'gitBranch: ' + gitBranch
                    echo 'tagName: ' + tagName
                }                
            }
        }
    }
}