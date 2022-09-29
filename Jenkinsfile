def secret = 'akmalll'
def server = 'apps@103.174.114.24'
def directory = 'literature-frontend'
def branch = 'production'

pipeline{
        agent any
        stages{
                stage ('Delete container and images & git pull'){
                        steps{
                                sshagent([secret]) {
                                        sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                                        cd ${dir}
                                        #docker-compose down
                                        #docker system prune -f
                                        git pull origin1 ${branch}
                                        exit
                                        EOF"""
                                }
                        }
                }
        stage ('Build Images'){
                        steps{
                                sshagent([secret]) {
                                        sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                                        cd ${dir}
                                        #docker build -t ${images} .
                                        exit
                                        EOF"""
                                }
                        }
                }
        stage ('Create Container with compose'){
                        steps{
                                sshagent([secret]) {
                                        sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                                        cd ${dir}
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
