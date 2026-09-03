# AWS CloudFormation
AWS CloudFormation is a infrastructure as a Code[`JSON` or `YAML`] (IaC) service that lets you define, provisions, and manage AWS resources in a declarative, template based format.

Till now we were configuring our aws infrastructure manually, cloudofrmation help to create and manage them using code.

**Example** : Code to create EC2 Instance
```yaml
Resources:
  SimpleEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro
      ImageId: ami-0b79f6b294a030f24  # AMI ID for your region
      Tags:
        - Key: Name
          Value: MySimpleInstance
```


### Get Started
Need to create Stack
- Existing Templates : Choose code from S3 file, File Upload, Sync with Git
- Create Stack Name  >  Submit
- Select the Stack > Resource > Get the Created EC2 Instance from declarative

**Change in Instance** :
- Select the Stack > Update 
- Either update to current template, or replace the template, or update using inFrastructure Composer UI

**Automatic Rollback** :
When update fails, cloudfrormation will rollback to the last state. It manages the state of the instance


### Change Set Update


### Why CloudFormation
- **Consistency**: IF you need to create instance multiple time over a period of time, can use these templates and create instance with same configuration easily
- **Automation**: Need to create instance automatically or based on certain conditions without creating instance each time manually
- **Repeatable**: Replicate environment easily


### Key Points
- Infrastructure as Code (IaC) : Automates the creation management, and updating AWS infrastructure.
- Declarative Language : 
- Template-Based : uses JSON or YAML template to specify AWS resources and configurations.
- State Management : Manges state internally, eliminating the need for seperate state files.
- Stacks and Stack Sets : Organize resources in stacks for eaiser management and allow for multi-accouunt and region development with stack sets.
- Cost-Free Tool : Cloudformation itself is free, you only pay for the resources created.


### Use Case
- Create EC2 Instacne with ElasticIp and Security Groups
- Provision S3 Bucket with HTTP endpoints
- Set up VPCs with subnet and router tables
- Create RDS database with automatic backups
- Deploy Lambda Function with API Gateway integrations
- Set up Elastic Load Balancer and Auto Scaling for web applications
- Deploy IAM roles, policies, and user access maangement

**Create Stack using CLI** : 
```bash
aws cloudformation create-stack --stack-name MyCLIStackOne --template-body file://path/to/template.yaml
```

#### Resource 
https://docs.aws.amazon.com/cloudformation/