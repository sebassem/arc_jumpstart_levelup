# Azure Arc-enabled Servers - #FollowAlong session at PowerShell Conference Europe 2024

## Goals

In this immersive 90-minute session, participants will embark on a hands-on journey to explore the capabilities of Azure Arc. Utilizing the Azure Arc Jumpstart resources, attendees will gain a deep understanding of how Azure Arc functions and its applications in a real-world scenario. The session is crafted to encourage active participation, enabling attendees to not only listen but also engage directly with the material through hands-on labs.

After completion of this session, you will be able to:

- Understand pre-requisites to onboard Windows and Linux servers to Azure Arc
- Onboard Windows and Linux servers running using different onboarding methods
- Monitor your Azure Arc-enabled servers using Azure Monitor
- Keep your Azure Arc-enabled servers patched using Azure Update Manager
- Monitor changes to your Azure Arc-enabled servers using Change tracking and inventory
- SSH into your Azure Arc-enabled servers using SSH access
- Run scripts in your Arc-enabled Windows server by using Run Commands
- Configure your Azure Arc-enabled servers using Azure Automanage machine configuration
- Manage the Windows operating system of your Arc-enabled servers using Windows Admin Center (Preview)
- Query and inventory your Azure Arc-enabled servers using Azure Resource Graph
- Enforce governance across your Azure Arc-enabled servers using Azure Policy

| Module | Duration | Facilitator |
|---------------|---------------|---------------|
|[**Understand pre-requisites to onboard Windows and Linux servers to Azure Arc**](#levelup-lab-environment) | 5 minutes | Seif Bassem |
|[**1 - Onboard Windows and Linux servers running using different onboarding methods**](#module-1-on-boarding-to-azure-arc-enabled-servers) | 10 minutes | Jan Egil Ring |
|[**2 - Query and inventory your Azure Arc-enabled servers using Azure Resource Graph**](#module-11-query-and-inventory-your-azure-arc-enabled-servers-using-azure-resource-graph) | 5 minutes | Seif Bassem |
|[**3 - Monitor your Azure Arc-enabled servers using Azure Monitor, Change Tracking and Inventoryr**](#module-2-monitor-your-azure-arc-enabled-servers-using-azure-monitor) | 20 minutes | Seif Bassem |
|[**4 - SSH into your Azure Arc-enabled servers using SSH access**](#module-7-ssh-into-your-azure-arc-enabled-servers-using-ssh-access) | 10 minutes | Jan Egil Ring |
|[**5 - Keep your Azure Arc-enabled servers patched using Azure Update Manager**](#module-5-keep-your-azure-arc-enabled-servers-patched-using-azure-manager) | 10 minutes | Seif Bassem |
|**6 Run scripts in your Arc-enabled Windows server by using Run Commands** | 5 minutes | Jan Egil Ring |
|[**7 - Configure your Azure Arc-enabled servers using Azure Automanage machine configuration**](#module-9-configure-your-azure-arc-enabled-servers-using-azure-automanage-machine-configuration) | 15 minutes | Jan Egil Ring |
|[**8 - Manage the Windows operating system of your Arc-enabled Servers using Windows Admin Center (Preview)**](#module-10-manage-your-azure-arc-enabled-servers-using-admin-center-preview) | 5 minutes | Seif Bassem |

## LevelUp lab guidance

There are two ways to get access to the lab modules and guidance.

1. You can use this GitHub repository.
2. You can open the guide using VSCode inside the ArcBox-Client VM to walk you through each module of this levelup.

## Lab Environment

ArcBox PSConfEU edition is a special “flavor” of ArcBox that is intended for users who want to experience Azure Arc-enabled servers' capabilities in a sandbox environment. Screenshot below shows layout of the lab environment.

  ![Screenshot showing ArcBox architecture](ArcBox-architecture.png)

### Prerequisites

- [Install or update Azure CLI to version 2.51.0 and above](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Use the below command to check your current installed version.

  ```shell
  az --version
  ```

    ![Screenshot showing azure cli version](./azcli_version.png)

- Login to AZ CLI using the ```az login``` command.

  ```shell
  az login
  ```

If you have access to multiple tenants, use the `--tenant` switch.

  ```shell
  az login --tenant <tenantId>
  ```

- [Install or update Azure PowerShell](https://learn.microsoft.com/powershell/azure/install-azps-windows). Use the below command to check if its installed.

  ```shell
  Get-Module -Name Az.Accounts -listavailable
  ```

- Login to Azure PowerShell the ```Connect-AzAccount``` command.

  ```shell
  Connect-AzAccount
  ```

- Set the default subscription using Azure CLI.

  ```shell
  $subscriptionId = "<Subscription Id>"
  az account set -s $subscriptionId
  ```

- Set the default subscription using Azure PowerShell.

  ```shell
  $subscriptionId = "<Subscription Id>"
  Set-AzContext -SubscriptionId $subscriptionId
  ```

- Ensure that you have selected the correct subscription you want to deploy ArcBox to by using the ```az account list --query "[?isDefault]"``` command. If you need to adjust the active subscription used by Az CLI, follow [this guidance](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription).

- ArcBox must be deployed to one of the following regions. **Deploying ArcBox outside of these regions may result in unexpected results or deployment errors.**

  - East US
  - East US 2
  - Central US
  - West US 2
  - North Europe
  - West Europe
  - France Central
  - UK South
  - Australia East
  - Japan East
  - Korea Central
  - Southeast Asia

- **ArcBox requires 16 DSv5-series vCPUs** when deploying with default parameters such as VM series/size. Ensure you have sufficient vCPU quota available in your Azure subscription and the region where you plan to deploy ArcBox. You can use the below Az CLI command to check your vCPU utilization.

  ```shell
  az vm list-usage --location <your location> --output table
  ```

  ![Screenshot showing az vm list-usage](./azvmlistusage.png)

- Register necessary Azure resource providers by running the following commands.

  ```shell
  az provider register --namespace Microsoft.HybridCompute --wait
  az provider register --namespace Microsoft.GuestConfiguration --wait
  az provider register --namespace Microsoft.AzureArcData --wait
  az provider register --namespace Microsoft.OperationsManagement --wait
  az provider register --namespace Microsoft.SecurityInsights --wait
  ```

- To deploy ArcBox, an Azure account assigned with the _Owner_ Role-based access control (RBAC) role is required.

    > **NOTE: The Jumpstart scenarios are designed with as much ease of use in-mind and adhering to security-related best practices whenever possible. It is optional but highly recommended to scope the access to a specific [Azure subscription and resource group](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest) as well considering using a [less privileged service principal account](https://docs.microsoft.com/azure/role-based-access-control/best-practices)**

### Deployment

#### Deployment Option 1: Azure portal

- Click the <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Farc_jumpstart_levelup%2Fpsconfeu%2Fazure_arc_servers_jumpstart%2FARM%2Fazuredeploy.json" target="_blank"><img src="https://aka.ms/deploytoazurebutton"/></a> button and enter values for the the ARM template parameters.

  ![Screenshot showing Azure portal deployment of ArcBox](./portaldeploy.png)

  ![Screenshot showing Azure portal deployment of ArcBox](./portaldeployinprogress.png)

  ![Screenshot showing Azure portal deployment of ArcBox](./portaldeploymentcomplete.png)

    > **NOTE: The deployment takes around 10 minutes to complete.**

    > **NOTE: If you see any failure in the deployment, please check the [troubleshooting guide](https://azurearcjumpstart.io/azure_jumpstart_arcbox/itpro/#basic-troubleshooting).**

### Deployment Option 2: Bicep deployment via Azure CLI

- Clone the Azure Arc Jumpstart repository

  ```shell
  $folderPath = <Specify a folder path to clone the repo>

  Set-Location -Path $folderPath
  git clone https://github.com/azure/arc_jumpstart_levelup.git
  git checkout psconfeu
  Set-Location -Path "azure_arc\azure_jumpstart_arcbox_servers_levelup\bicep"
  ```

- Upgrade to latest Bicep version

  ```shell
  az bicep upgrade
  ```

- Edit the [main.parameters.json](https://github.com/Azure/arc_jumpstart_levelup/blob/main/azure_arc_servers_jumpstart/bicep/main.parameters.json) template parameters file and supply some values for your environment.
  - _`spnTenantId`_ - Your Azure tenant id
  - _`windowsAdminUsername`_ - Client Windows VM Administrator name
  - _`windowsAdminPassword`_ - Client Windows VM Password. Password must have 3 of the following: 1 lower case character, 1 upper case character, 1 number, and 1 special character. The value must be between 12 and 123 characters long.
  - _`logAnalyticsWorkspaceName`_ - Unique name for the ArcBox Log Analytics workspace
  - _`emailAddress`_ - Your email address, to configure alerts for the monitoring action group

  ![Screenshot showing example parameters](./parameters_bicep.png)

- Now you will deploy the Bicep file. Navigate to the local cloned [deployment folder](https://github.com/Azure/arc_jumpstart_levelup/blob/main/azure_arc_servers_jumpstart/bicep) and run the below command:

  ```shell
  az group create --name "<resource-group-name>" --location "<preferred-location>"
  az deployment group create -g "<resource-group-name>" -f "main.bicep" -p "main.parameters.json"
  ```

    > **NOTE: If you see any failure in the deployment, please check the [troubleshooting guide](https://azurearcjumpstart.io/azure_jumpstart_arcbox/itpro/#basic-troubleshooting).**

    > **NOTE: The deployment takes around 20 minutes to complete.**

### Connecting to the ArcBox Client virtual machine

Various options are available to connect to _ArcBox-Client_ VM, depending on the parameters you supplied during deployment.

- [RDP](https://azurearcjumpstart.io/azure_jumpstart_arcbox/ITPro/#connecting-directly-with-rdp) - available after configuring access to port 3389 on the _ArcBox-NSG_, or by enabling [Just-in-Time access (JIT)](https://azurearcjumpstart.io/azure_jumpstart_arcbox/ITPro/#connect-using-just-in-time-accessjit).
- [Azure Bastion](https://azurearcjumpstart.io/azure_jumpstart_arcbox/ITPro/#connect-using-azure-bastion) - available if ```true``` was the value of your _`deployBastion`_ parameter during deployment.

#### Connecting directly with RDP

By design, ArcBox does not open port 3389 on the network security group. Therefore, you must create an NSG rule to allow inbound 3389.

- Open the _ArcBox-NSG_ resource in Azure portal and click "Add" to add a new rule.

  ![Screenshot showing ArcBox-Client NSG with blocked RDP](./rdp_nsg_blocked.png)

  ![Screenshot showing adding a new inbound security rule](./nsg_add_rule.png)

- Specify the IP address that you will be connecting from and select RDP as the service with "Allow" set as the action. You can retrieve your public IP address by accessing [https://icanhazip.com](https://icanhazip.com) or [https://whatismyip.com](https://whatismyip.com).

  <img src="./nsg_add_rdp_rule.png" alt="Screenshot showing adding a new allow RDP inbound security rule" width="400">

  ![Screenshot showing all inbound security rule](./rdp_nsg_all_rules.png)

  ![Screenshot showing connecting to the VM using RDP](./rdp_connect.png)

#### Connect using Azure Bastion

- If you have chosen to deploy Azure Bastion in your deployment, use it to connect to the VM.

  ![Screenshot showing connecting to the VM using Bastion](./bastion_connect.png)

  > **NOTE: When using Azure Bastion, the desktop background image is not visible. Therefore some screenshots in this guide may not exactly match your experience if you are connecting to _ArcBox-Client_ with Azure Bastion.**

#### The Logon scripts

- Once you log into the _ArcBox-Client_ VM, multiple automated scripts will open and start running. These scripts usually take 10-20 minutes to finish, and once completed, the script windows will close automatically. At this point, the deployment is complete.

  ![Screenshot showing ArcBox-Client](./automation.png)

- Deployment is complete! Let's begin exploring the features of Azure Arc-enabled servers with the lab modules.

  ![Screenshot showing complete deployment](./arcbox_complete.png)

  ![Screenshot showing ArcBox resources in Azure portal](./rg_arc.png)

## Modules

### Module 1: On-boarding to Azure Arc-enabled servers

#### Objective

The deployment process that you have walked through in Lab01 should have set up four VMs running on Hyper-V in the ArcBox-Client machine. Two of these machines have been connected to Azure Arc for you by the set script. In this exercise you will verify that these two machines are indeed Arc-enabled and you will identify the other two machines that you will Arc-enable.

##### Task 1: Use the Azure portal to examine you Arc-enabled machines inventory

1. Enter "Machines - Azure Arc" in the top search bar in the Azure portal and select it from the displayed services.

![!Screenshot Arc_servers_search](./Arc_servers_search.png)

2. You should see the machines that are connected to Arc already: Arcbox-Ubuntu-01, ArcBox-Win2k12 and ArcBox-Win2K19.

![Screenshot showing existing Arc connected servers](./First_view_of_Arc_connected.png)

##### Task 2:  Examine the virtual machines that you will Arc-enable

1. For the follow-along session you want to connect one of the other available machines running as VMs in the ArcBox-Client. You can see these (ArcBox-Win2K22 and ArcBox-Ubuntu-02) by running the Hyper-V Manager in the ArcBox-Client (after you have connected to it with RDP as explained earlier in Lab01).

![Screenshot of 4 machines on Hyper-v](./choose_Hyper-V.png)

##### Task 3: Installation and registration of the Azure Arc connected machine agent for a Windows machine

1. From the Azure portal go to the "Machines - Azure Arc" page and select "Add/Create" at the upper left, then select "Add a machine".

![Screenshot to select add a machine](./Select_Add_a_machine.png)

2. In the next screen, go to "Add a single sever" and click on "Generate script".

3. Fill in the Resource Group, Region, Operating System (Windows), keep Connectivity as "Public endpoint". Then download the script to your local machine (or you can copy the content into the clipboard).

4. Go to the ArcBox-Client machine via RDP and from Hyper-V manager right-click on the ArcBox-Win2K22 VM and click "Connect" (Administrator default password is ArcDemo123!!). Then start PowerShell in the ArcBox-Win2K22 VM and copy the content of the onboarding script into the terminal.

![Screenshot run onboard windows script](./run_windows_onboard_script.png)

6. On successful completion a message is displayed to confirm the machine is connected to Azure Arc. We can also verify that our Windows machine is connected in the Azure portal (Machines - Azure Arc).

![Screenshot confirm win machine on-boarded](./confirm_windows_machine_onboarding.png)

For more information about deployment options, see the following two articles:
- [Azure Connected Machine agent deployment options](https://learn.microsoft.com/azure/azure-arc/servers/deployment-options)
- [Connect hybrid machines to Azure at scale](https://learn.microsoft.com/azure/azure-arc/servers/onboard-service-principal).

### Module 2: Query and inventory your Azure Arc-enabled servers using Azure Resource Graph

#### Objective

In this exercise, you will learn how to use the Azure Resource queries both in the Azure Graph Explorer and Powershell to demonstrate inventory management of your Azure Arc connected servers. Note that the results you get by running the graph queries in this module might be different from the sample screenshots as your environment might be different.

##### Task 1: Apply resource tags to Azure Arc-enabled servers

In this first step, you will assign Azure resource tags to some of your Azure Arc-enabled servers. This gives you the ability to easily organize and manage server inventory.

- Enter "Machines - Azure Arc" in the top search bar in the Azure portal and select it from the displayed services.

    ![Screenshot showing how to display Arc connected servers in portal](./Arc_servers_search.png)

- Click on the Windows 2019 Azure Arc-enabled server.

    ![Screenshot showing existing Arc connected servers](./click_on_any_arc_enabled_server.png)

- Click on "Tags". Add a new tag with Name="Scenario” and Value="azure_arc_servers_inventory”. Click Apply when ready.

    ![Screenshot showing adding tag to a server](./tagging_servers.png)

- Repeat the same process in other Azure Arc-enabled servers if you wish. This new tag will be used later when working with Resource Graph Explorer queries.

##### Task 2: The Azure Resource Graph Explorer

- Now, we will explore our hybrid server inventory using a number of Azure Graph Queries. Enter "Resource Graph Explorer" in the top search bar in the Azure portal and select it.

    ![Screenshot of Graph Explorer in portal](./search_graph_explorer.png)

- The scope of the Resource Graph Explorer can be set as seen below

    ![Screenshot of Graph Explorer Scope](./Scope_of_Graph_Query.png)

##### Task 3: Run a query to show all Azure Arc-enabled servers in your subscription

- In the query window, enter and run the following query and examine the results which should show your Arc-enabled servers. Note the use of the KQL equals operator (=~) which is case insensitive [KQL =~ (equals) operator](https://learn.microsoft.com/azure/data-explorer/kusto/query/equals-operator).

    ```shell
    Resources
    | where type =~ 'Microsoft.HybridCompute/machines'
    ```

    ![Screenshot of query to list arc servers](./query_arc_machines.png)

- Scroll to the right on the results pane and click "See Details" to see all the Azure Arc-enabled server metadata. Note for example the list of detected properties, we will be using these in the next task.

- You can also run the same query using PowerShell (e.g. using Azure Cloud Shell) providing that you have added the required module "Az.ResourceGraph" as explained in [Run your first Resource Graph query using Azure PowerShell](https://learn.microsoft.com/azure/governance/resource-graph/first-query-powershell#add-the-resource-graph-module).

To install the PowerShell module, run the following command

  ```powershell
  Install-Module -Name Az.ResourceGraph
  ```

Then run the query in PowerShell

  ```powershell
  Search-AzGraph -Query "Resources | where type =~ 'Microsoft.HybridCompute/machines'"
  ```

##### Task 4: Query your server inventory using the available metadata

- Use PowerShell and the Resource Graph Explorer to summarize the server count by "logical cores" which is one of the detected properties referred to in the previous task. Remember to only use the query string, which is enclosed in double quotes, if you want to run the query in the portal.

    ```powershell
    Search-AzGraph -Query  "Resources
    | where type =~ 'Microsoft.HybridCompute/machines'
    | extend logicalCores = tostring(properties.detectedProperties.logicalCoreCount)
    | summarize serversCount = count() by logicalCores"
    ```

- The Graph Explorer allows you to get a graphical view of your results by selecting the "charts" option.

    ![Screenshot of the logicalCores server summary](./chart_for_vcpu_summay.png)

##### Task 5: Use the resource tags in your Graph Query

- Let’s now build a query that uses the tag we assigned earlier to some of our Azure Arc-enabled servers. Use the following query that includes a check for resources that have a value for the "Scenario" tag. Feel free to use the portal or PowerShell. Check that the results match the servers that you set tags for earlier.

    ```powershell
    Search-AzGraph -Query  "Resources
    | where type =~ 'Microsoft.HybridCompute/machines' and isnotempty(tags['Scenario'])
    | extend Scenario = tags['Scenario']
    | project name, tags"
    ```

##### Task 6: List the extensions installed on the Azure Arc-enabled servers

- Run the following advanced query which allows you to see what extensions are installed on the Arc-enabled servers. Notice that running the query in PowerShell requires us to escape the $ character as explained in [Escape Characters](https://learn.microsoft.com/azure/governance/resource-graph/concepts/query-language#escape-characters).

    ```powershell
    Search-AzGraph -Query "Resources
    | where type == 'microsoft.hybridcompute/machines'
    | project id, JoinID = toupper(id), ComputerName = tostring(properties.osProfile.computerName), OSName = tostring(properties.osName)
    | join kind=leftouter(
        Resources
        | where type == 'microsoft.hybridcompute/machines/extensions'
        | project MachineId = toupper(substring(id, 0, indexof(id, '/extensions'))), ExtensionName = name
    ) on `$left.JoinID == `$right.MachineId
    | summarize Extensions = make_list(ExtensionName) by id, ComputerName, OSName
    | order by tolower(OSName) desc"
    ```

- If you have used the portal to run the query then you should remove the escape from the $ character ($left.JohnID and $right.MachinID) before running the query as shown in the screenshot below.

    ![Screenshot of extensions query](./Extensions_query.png)

##### Task 7: Query other properties

- Azure Arc provides additional properties on the Azure Arc-enabled server resource that we can query with Resource Graph Explorer. In the following example, we list some of these key properties, like the Azure Arc Agent version installed on your Azure Arc-enabled servers

    ```powershell
    Search-AzGraph -Query  "Resources
    | where type =~ 'Microsoft.HybridCompute/machines'
    | extend arcAgentVersion = tostring(properties.['agentVersion']), osName = tostring(properties.['osName']), osVersion = tostring(properties.['osVersion']), osSku = tostring(properties.['osSku']),
    lastStatusChange = tostring(properties.['lastStatusChange'])
    | project name, arcAgentVersion, osName, osVersion, osSku, lastStatusChange"
    ```

- Running the same query in the portal should result in something like the following

    ![Screenshot of extra properties](./extra_properties.png)