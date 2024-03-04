pipeline{
    agent any
    environment{
        cred = credentials('aws')
    }
    stages{
        stage('test'){
            steps{
                script{
                    def id = sh(script:"aws ec2 describe-instances --region 'us-east-1' --filters 'Name=tag:Name,Values=Jenkins*' 
                    --query 'Reservations[*].Instances[*].[InstanceId]' --output text", returnStdout: true).trim()
                    echo "${id}"
                    def volumeId = sh(script: "aws ec2 describe-instances --region 'us-east-1' --instance-ids '${id}' 
                    --query 'Reservations[*].Instances[*].BlockDeviceMappings[0].Ebs.VolumeId' --output text", returnStdout: true).trim()
                    echo "${volumeId}"
                    def snapshotId = sh(script: "aws ec2 create-snapshot --region 'us-east-1' --volume-id '${volumeId}' --query 
                    'SnapshotId' --output text", returnStdout: true).trim()
                    echo "${snapshotId}"
                }    
            }
        }
    }
}
