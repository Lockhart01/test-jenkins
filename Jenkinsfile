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
        stage('storagePlugin only'){
            agent{
                label 'slave'
            }
            when{
                expression { env.BRANCH_NAME == "storagePlugin" }
            }
            steps{	        	
		        unstash 'app'
		        sh 'tar -czvf myapp-${NAME}.tar.gz ${WORKSPACE}/myapp'
                sh 'ls -l'
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'https',
                    nexusUrl: '192.168.100.12:8081',
                    groupId: '',
                    version: "${env.BRANCH_NAME}-${NAME}",
                    repository: 'myapp-plugin',
                    credentialsId: 'nexus-creds',
                    artifacts: [
                        [artifactId: "myapp-${env.BRANCH_NAME}-${NAME}",
                        classifier: '',
                        file: '${WORKSPACE}/myapp-${NAME}.tar.gz',
                        type: 'tar.gz']
                    ]
                );				
	        }
        }
    }
}
