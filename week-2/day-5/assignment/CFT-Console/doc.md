## CloudFormation Deployment via AWS Management Console

![IMAGE](../CF-Console/images/image-7.png)

### Step 1: Uploading and Validating the CloudFormation Template

In this step, a CloudFormation stack was initiated by uploading a YAML template through the AWS Management Console. The uploaded template defines the complete infrastructure required for the deployment, including networking components, security configurations, and compute resources.

![IMAGE](../CF-Console/images/image-1.png)

---

### Step 2: Configuring Stack Parameters – EC2 Key Pair

During this step, the required CloudFormation stack parameter was configured. The template defines a parameter named `KeyPair`, which is used to specify an existing EC2 key pair for secure SSH access to the instance.

![IMAGE](../CF-Console/images/image-2.png)

---

### Step 3: Providing Stack Details and Parameter Values

In this step, the CloudFormation stack details were specified using the AWS Management Console. A descriptive stack name, **`EC2-stack`**, was assigned to uniquely identify the deployment. Additionally, the required stack parameter was configured by selecting an existing EC2 key pair.

![IMAGE](../CF-Console/images/image-3.png)

---

### Step 4: Verifying Stack Creation and Resource Provisioning

This step shows the CloudFormation stack **`EC2-stack`** in a `CREATE_COMPLETE` state, confirming that all resources defined in the template were successfully created. The **Resources** tab displays the logical IDs, physical resource IDs, and creation status for each AWS resource.

![IMAGE](../CF-Console/images/image-4.png)

---

### Step 5: Securing the EC2 Private Key File

This step shows the verification and modification of file permissions for the EC2 private key file (`stack-key.pem`) on the local system. Initially, the key file had overly permissive access rights, which could prevent SSH from using the key securely.

![IMAGE](../CF-Console/images/image-5.png)

---

### Step 6: Accessing the EC2 Instance via SSH

This step demonstrates successful SSH access to the EC2 instance created through CloudFormation. The connection was initiated from the local system using the private key file associated with the EC2 key pair and the Elastic IP assigned to the instance.

![IMAGE](../CF-Console/images/image-6.png)

---