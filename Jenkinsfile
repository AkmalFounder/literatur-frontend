def secret = 'akmalll'
def server = 'apps@103.174.114.24'
def directory = 'literature-frontend'
def branch = 'production'

pipeline{
    agent any
    stages{
        stage ('Docker compose first'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose down
                    docker system prune -f
                    git pull origin ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage ('docker build'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose build
                    exit
                    EOF"""
                }
            }
        }
        stage ('up container'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose up -d
                    exit
                    EOF"""
                }
            }
        }
        stage ('Send Notification'){
           steps{
                discordSend description: 'Frontend Pipeline Succesfull', footer: '', image: '', link: '', result: '', scmWebUrl: '', thumbnail: '', title: 'Jenkins Notif',
                webhookURL: 'https://discord.com/api/webhooks/1020148936972968028/h0Zt34JAcxRW36OaKkxAV1K_bmYyp7L5XVIX13yZ-nUzD9Lo6dTwsLubxNCkqurB_cOB'
		}
	   }
    }
}
