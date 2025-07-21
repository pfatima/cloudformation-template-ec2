pipeline {
    agent any

    parameters {
        string(name: 'STACK_NAME', defaultValue: 'ec2-stack', description: 'CloudFormation Stack Name')
        string(name: 'TEMPLATE_FILE', defaultValue: 'ec2-instance.yml', description: 'CloudFormation Template File')
        string(name: 'INSTANCE_NAME', defaultValue: 'fatima-ec2-instance', description: 'EC2 Instance Name')
        choice(name: 'REGION', choices: ['us-east-1', 'us-west-1', 'ap-south-1'], description: 'AWS Region')
    }

    environment {
        AWS_DEFAULT_REGION = "${params.REGION}"
    }

    stages {
        stage('Checkout Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/pfatima/cloudformation-template-ec2.git'
            }
        }

        stage('Deploy EC2 Stack') {
            steps {
                script {
                    echo "Running CloudFormation deployment for EC2..."
                    sh """
                        aws cloudformation deploy \
                          --stack-name ${params.STACK_NAME} \
                          --template-file ${params.TEMPLATE_FILE} \
                          --region ${params.REGION} \
                          --parameter-overrides InstanceName=${params.INSTANCE_NAME}
                    """
                }
            }
        }
    }
}
