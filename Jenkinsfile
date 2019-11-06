pipeline
{
    agent any
    stages
    {
        stage('Init')
        {
            steps
            {
                echo "Testing ..."
            }
        }
        stage('Build')
        {
			steps
			{
				bat "mvn clean package"
			}
			post
			{
				success
				{
					echo "Now Archiving ..."
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
        stage('Deploy to Staging')
        {
            steps
            {
                echo "Code Deployed."
				build job: 'deploy_to_staging'
            }
        }
		stage('Deploy to Production')
		{
			steps
			{
				timeout(time:5, unit:'DAYS')
				{
					input message:'Approve PRODUCTION Deployement ?'
				}
				build job: 'deploy_to_prod'
			}
			post
			{
				success
				{
					echo 'Code Deployed To Production'
				}
				failure
				{
					echo 'Depoyement failed'
				}				
			}
		}
    }
}