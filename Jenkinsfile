pipeline{
    agent any
    stages{
	stage('Prepare'){
		steps{
			script{
				Date date = new Date()
				env.DATE= date.format("yyyyMMdd")
			}
			
		}
		post{
				always{
					script{
						currentBuild.displayName = "myapp-${DATE}"
					}
				}
			}
	    }
        stage('Use'){
            agent{
                dockerfile{
                    dir 'docker'
                    label 'docker'
                }
            }
            steps{
                sh 'echo $(fortune|cowsay)'
                
            }
        }
       
    }
}
