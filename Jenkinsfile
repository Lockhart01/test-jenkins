pipeline{
    agent any
    tools{
        maven 'M3'
    }
    stages{
        stage('Build'){
            agent{
                label 'node2'
            }
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'WipeWorkspace'],[$class: 'LocalBranch', localBranch: "**"]], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Lockhart01/test-jenkins']]])
                echo "${env.BRANCH_NAME}"
                sh 'cd myapp && mvn clean package'
                stash includes: '**/target/*.jar', name: 'app' 
            }
        }
        stage('dev only'){
            agent{
                label 'docker'
            }
            when{
                expression { env.BRANCH_NAME == "dev" }
            }
            steps{
                sh 'echo "if you can see this, it means i was triggered from dev branch"'
            }
        }
        stage('storage'){
            agent{
                label 'docker'
            }
            steps{
                unstash 'app'
                archiveArtifacts artifacts: 'myapp/target/*.jar', followSymlinks: false
            }
        }
    }
}
