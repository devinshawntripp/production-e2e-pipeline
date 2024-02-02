pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                echo "========executing Cleanup Workspace========"
                cleanWs()

            }
        }
        stage("Checkout from SCM"){
            steps{
                echo "========Checkout from SCM========"
                git branch: 'main', credentialId: 'github', url: 'https://github.com/devinshawntripp/production-e2e-pipeline'

            }
        }
    }

}