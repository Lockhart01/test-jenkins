pipeline{
    agent any
    environment{
        JAVA_TOOL_OPTIONS = '-Duser.home=/tmp'
    }    
    stages{
        stage('Build'){
            agent{
                docker{
                    image 'maven:3.5.2-jdk-7-alpine'
                    label 'node1'
                }
            }
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'WipeWorkspace'],[$class: 'LocalBranch', localBranch: "**"]], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Lockhart01/test-jenkins']]])
                echo "${env.BRANCH_NAME}"
                sh 'cd myapp && mvn clean package'
                stash includes: '**/target/*.jar', name: 'app' 
            }
        }
        stage('Build #2'){
            agent{
                docker{
                    image 'maven:3.5.2-jdk-7-alpine'
                    args '-v m2:/tmp'
                    label 'node1'
                }
            }
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'WipeWorkspace'],[$class: 'LocalBranch', localBranch: "**"]], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Lockhart01/test-jenkins']]])
                echo "${env.BRANCH_NAME}"
                sh 'cd myapp && mvn clean package'
            }
        }
        stage('storage'){
            steps{
                unstash 'app'
                archiveArtifacts artifacts: 'myapp/target/*.jar', followSymlinks: false
            }
        }
    }
}
