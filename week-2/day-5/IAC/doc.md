## EC2 Deployment Using AWS CloudFormation – Explained

### 1. Purpose of the Template

This CloudFormation template automatically deploys a complete EC2 environment that includes:

1. A custom VPC
2. A public subnet
3. Internet access (IGW + route table)
4. A security group (SSH + HTTP)
5. An EC2 instance
6. A public Elastic IP

This is an Infrastructure as Code (IaC) approach, meaning the entire setup is repeatable and version-controlled.

---

### 2. Template Metadata

    AWSTemplateFormatVersion: "2010-09-09"
    Description: CloudFormation template to create a VPC, IG, subnet, route table, SG and EC2 instance.

- AWSTemplateFormatVersion → CloudFormation format version
- Description → Human-readable explanation of what the template does

---

### 3. Parameters Section

    Parameters:
    KeyPair:
        Type: AWS::EC2::KeyPair::KeyName
        Description: Existing Key pair name

**What this does?**
- Asks the user to select an existing EC2 Key Pair
- This key pair is required to SSH into the EC2 instance

**Why parameters are used**
- Makes the template reusable
- Avoids hard-coding sensitive or environment-specific values

---

### 4. Networking Resources (VPC & Subnet)

### 4.1 VPC

    VPC:
    Type: AWS::EC2::VPC
    Properties:
        CidrBlock: 10.0.0.0/16
        EnableDnsSupport: true
        EnableDnsHostnames: true

**Explanation**
- Creates a Virtual Private Cloud
- IP range: 10.0.0.0 – 10.0.255.255
- DNS support is enabled so EC2 instances can:
    - Resolve domain names
    - Get public DNS names

---

### 4.2 Subnet

    Subnet:
    Type: AWS::EC2::Subnet
    Properties:
        VpcId: !Ref VPC
        CidrBlock: 10.0.0.0/24
        AvailabilityZone: us-east-1a

**Explanation**
- Creates a public subnet inside the VPC
- IP range: 10.0.0.0 – 10.0.0.255
- Placed in Availability Zone us-east-1a
- EC2 will be launched inside this subnet

---

### 5. Internet Connectivity

### 5.1 Internet Gateway (IGW)

    InternetGateway:
    Type: AWS::EC2::InternetGateway

- Allows the VPC to communicate with the internet

---

### 5.2 Attach IGW to VPC

    VPCGatewayAttachment:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
        VpcId: !Ref VPC
        InternetGatewayId: !Ref InternetGateway

- Physically attaches the IGW to the VPC
- Without this, internet access is impossible

---

### 6. Routing (Public Internet Access)

### 6.1 Route Table

    RouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
        VpcId: !Ref VPC

- Defines how traffic is routed inside the VPC

### 6.2 Default Route to Internet

    Route:
    Type: AWS::EC2::Route
    DependsOn: VPCGatewayAttachment
    Properties:
        RouteTableId: !Ref RouteTable
        DestinationCidrBlock: 0.0.0.0/0
        GatewayId: !Ref InternetGateway

**Explanation**
- 0.0.0.0/0 = all internet traffic
- Sends traffic to the Internet Gateway
- DependsOn ensures IGW is attached first

---

### 6.3 Associate Route Table with Subnet

    SubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
        SubnetId: !Ref Subnet
        RouteTableId: !Ref RouteTable

- This is what makes the subnet public

---

### 7. Security Group (Firewall)

    SecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
        GroupDescription: Allow SSH and HTTP
        VpcId: !Ref VPC
        SecurityGroupIngress:
        - IpProtocol: tcp
            FromPort: 22
            ToPort: 22
            CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
            FromPort: 80
            ToPort: 80
            CidrIp: 0.0.0.0/0

**Allowed Traffic**
    Port	Purpose
    22	    SSH access
    80	    HTTP web traffic

**Note: 0.0.0.0/0 allows traffic from anywhere (acceptable for labs, not production)**

---

### 8. EC2 Instance

    EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
        ImageId: ami-068c0051b15cdb816
        InstanceType: t3.micro
        KeyName: !Ref KeyPair
        SubnetId: !Ref Subnet
        SecurityGroupIds:
        - !Ref SecurityGroup

**Explanation**
- AMI: Amazon Linux 2 (us-east-1)
- Instance type: t3.micro (Free Tier eligible)
- Key pair: Used for SSH login
- Placed in public subnet
- Protected by the security group

---

### 8.1 EC2 Tag

    Tags:
    - Key: Name
        Value: EC2-1

- Helps identify the instance in AWS Console

---

### 9. Elastic IP (Static Public IP)

    ElasticIP:
    Type: AWS::EC2::EIP
    Properties:
        Domain: vpc
        InstanceId: !Ref EC2Instance

**Why Elastic IP?**
- Ensures the public IP does not change
- Useful for:
    - SSH
    - Hosting websites
    - DNS mapping

---

### 10. Outputs Section

    Outputs:
    ElasticIP:
        Value: !GetAtt ElasticIP.PublicIp
    InstanceId:
        Value: !Ref EC2Instance

**What this gives you after deployment**
- Public IP address of the EC2
- Instance ID for management and reference

--- 

### 11. How You Access the EC2

    ssh -i key.pem ec2-user@<Elastic-IP>

--- 
