![ma1](https://github.com/Lofty900/new-task/assets/78558689/86cf89de-bb02-449c-b149-7f289900c469)

First, I unzipped the folder and extracted its contents. Then I opened it using VS Code.

**To fork, configure, and deploy the solution on your Azure cloud environment, follow these steps:**

**Step 1: Fork the Repository**

Navigate to the repository: kindly go to https://github.com/Lofty900/new-task.git
Click the "Fork" button at the top-right corner of the page.
Choose your GitHub account or organization to fork the repository.

**Step 2: Clone the Forked Repository**

Navigate to your forked repository on GitHub.
Click the "Code" button and copy the URL.
Clone the repository to your local machine:
sh
Copy code
git clone https://github.com/<your-username>/new-task.git

**Step 3: Configure Azure Pipelines**

Sign in to your Azure DevOps organization.
Create a new project if you don't already have one.
Navigate to the project and select "Pipelines" from the left sidebar.
Create a new pipeline and connect it to your GitHub repository.

**Configure the pipeline:**

Note: I added separate Docker files to the React front and Java Backend of the application to build the app into a container image. You'll create two pipelines,
one for the frontend and one for the backend.

For the backend pipeline, use azure-pipelines.backend.yml.
For the frontend pipeline, use azure-pipelines.frontend.yml.
For the infrastructure deployment, use terraform/azure-pipelines.yml.

**Step 4: Set Up Azure Resources**

Set up service connections in Azure DevOps to your Azure subscription:
Go to your Azure DevOps project settings.
Under "Pipelines", select "Service connections".
Add a new service connection for Azure Resource Manager and configure it with the appropriate permissions.

**Step 5: Update Pipeline Variables**
Update the necessary variables in your Azure Pipelines to match your environment. This includes:

Azure Container Registry service connection
Managed Identity
tenant ID
client ID
client secret
Azure subSscription
App service names
You can set these variables in the Azure DevOps pipeline UI under the "Variables" section for security reasons.

Note: If your service connections and MSI are not configured properly, Terraform will be unable to deploy the resources because of lack of authorization.

**Step 6: Deploy the Solution**

Run the infrastructure pipeline (terraform/azure-pipelines.yml) to create the Azure resources (ACR and AKS).
Run the backend pipeline (azure-pipelines.backend.yml) to build, test, and deploy the backend.
Run the frontend pipeline (azure-pipelines.frontend.yml) to build, test, and deploy the frontend.

**High-Level Documentation**
Overall Architecture
Repository Structure:

Application files for the Java backend and React frontend are stored in their respective directories.
CI/CD pipeline definitions are stored in YAML files in the root directory.
Terraform configurations for infrastructure deployment are stored in the terraform directory.
Infrastructure:

Azure Resource Group: A container for managing resources.
Azure Container Registry (ACR): Stores Docker images for the application.
Azure Kubernetes Service (AKS): Hosts the application, ensuring scalability and reliability.
Log Analytics Workspace: Monitors and collects logs from the AKS cluster.
Azure Monitor Diagnostic Settings: Enables detailed monitoring and auditing.
CI/CD Pipelines:

Backend Pipeline (azure-pipelines.backend.yml):
Builds and tests the Java backend.
Builds a Docker image and pushes it to ACR.
Deploys the backend to AKS.
Frontend Pipeline (azure-pipelines.frontend.yml):
Builds and tests the React frontend.
Builds a Docker image and pushes it to ACR.
Deploys the frontend to AKS.
Infrastructure Pipeline (terraform/azure-pipelines.yml):
Uses Terraform to deploy and manage Azure resources.
Provides cleanup functionality to destroy resources when needed.

**Scalability and Monitoring:**

The AKS cluster is configured to scale automatically based on demand.
Log Analytics and Azure Monitor are integrated for comprehensive monitoring and auditing.

**Access Control:**

Role-based access control (RBAC) is enabled for AKS.
Azure Active Directory integration ensures secure and managed access.
