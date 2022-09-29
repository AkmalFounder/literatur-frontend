def secret = 'akmalll'
def server = 'apps@103.174.114.24'
def directory = '/home/apps/literature-frontend'
def branch = 'production'
def images = 'yubisayu/literature-fe:1.0'

pipeline{
    agent any
    stages{
        stage ('docker compose'){
            steps{
                sshagent([credential]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker compose down
                    docker system prune -f
                    git pull origin ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage ('build docker starting'){
            steps{
                sshagent([credential]) {
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
                sshagent([credential]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker compose up -d
                    exit
                    EOF"""
                }
            }
        }
        stage ('push to docker hub'){
            steps{
                sshagent([credential]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker push ${images}
                    exit
                    EOF"""
               }
            }
        }
        stage ('Notif Discord & Jenkins'){
            steps{
                discordSend description: 'Pipeline Succesfull', footer: '', image: '', link: '', result: '', scmWebUrl: '', thumbnail: '', title: 'Jenkins Notification',
                    webhookURL: 'https://discord.com/api/webhooks/1020148936972968028/h0Zt34JAcxRW36OaKkxAV1K_bmYyp7L5XVIX13yZ-nUzD9Lo6dTwsLubxNCkqurB_cOB'
		}
	}
    }
}
