pipeline{
    agent any
    tools{
        maven 'M3'
    }
    stages{
	stage('Prepare'){
		steps{
			script{
				Random rnd = new Random()
				env.VERSION= rnd.nextInt(10)
				Date date = new Date()
				env.NAME= date.format("yyyyMMdd")
			}
			
		}
		post{
				always{
					script{
						currentBuild.displayName = "myapp-${NAME}"
					}
				}
			}
	    }
        stage('Build'){
            agent{
                label 'slave'
            }
            steps{

                checkout([$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'WipeWorkspace'],[$class: 'LocalBranch', localBranch: "**"]], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Lockhart01/test-jenkins']]])
                echo "${env.BRANCH_NAME}"
                sh 'cd myapp && mvn clean package'
                stash includes: '**/target/*.jar', name: 'app' 
            }
        }
        stage('storagePom only'){
            agent{
                label 'slave'
            }
            when{
                expression { env.BRANCH_NAME == "storagePom" }
            }
            steps{
	    	withCredentials([string(credentialsId: 'USER', variable: 'USER'), string(credentialsId: 'PASSWORD', variable: 'PASSWORD'), string(credentialsId: 'URL', variable: 'URL')]) {
	        	sh 'env'
                	sh 'cd myapp && mvn clean deploy -s ${WORKSPACE}/settings.xml '
		}
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
