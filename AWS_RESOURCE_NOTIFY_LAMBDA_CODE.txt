import boto3
from datetime import datetime

def get_ec2_instances():
    ec2 = boto3.resource('ec2')
    instances = ec2.instances.all()
    resources = []

    for instance in instances:
        resources.append({
            'Type': 'EC2 Instance',
            'Id': instance.id,
            'State': instance.state['Name']
        })

    return resources

def get_vpcs():
    ec2 = boto3.client('ec2')
    response = ec2.describe_vpcs()
    vpcs = response['Vpcs']
    resources = []

    for vpc in vpcs:
        resources.append({
            'Type': 'VPC',
            'Id': vpc['VpcId'],
            'State': vpc['State']
        })

    return resources

def get_auto_scaling_groups():
    autoscaling = boto3.client('autoscaling')
    response = autoscaling.describe_auto_scaling_groups()
    groups = response['AutoScalingGroups']
    resources = []

    for group in groups:
        resources.append({
            'Type': 'Auto Scaling Group',
            'Id': group['AutoScalingGroupName'],
            'State': group['Status']
        })

    return resources

def send_email(subject, body):
    sns = boto3.client('sns')

    # Replace with your SNS topic ARN
    topic_arn = 'arn:aws:sns:us-east-2:174321893572:daily_resource'

    message = f'Subject: {subject}\n\n{body}'

    sns.publish(
        TopicArn=topic_arn,
        Message=message
    )

def lambda_handler(event, context):
    current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

    # Get resources
    ec2_instances = get_ec2_instances()
    vpcs = get_vpcs()
    auto_scaling_groups = get_auto_scaling_groups()

    resources = ec2_instances + vpcs + auto_scaling_groups

    # Prepare summary
    summary = f'Resource Summary ({current_time}):\n\n'
    for resource in resources:
        summary += f'Type: {resource["Type"]}\n'
        summary += f'Id: {resource["Id"]}\n'
        summary += f'State: {resource["State"]}\n\n'

    # Send email
    subject = f'AWS Resource Summary ({current_time})'
    send_email(subject, summary)

    return {
        'statusCode': 200,
        'body': 'Email sent successfully'
    }
