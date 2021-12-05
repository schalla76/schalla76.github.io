# Azure Developer

## Help/Documentation Links

### Microsoft

- [Microsoft Help Links](https://docs.microsoft.com/en-us/azure/?product=featured)

### Sample code

- [SQL DB tutorial](https://github.com/Azure-Samples/dotnetcore-sqldb-tutorial)

### Sample Test Preparation

- [Exam Topics](https://examtopics.com)

## Powershell

### List the Az module

`Get-Module -Name Az -ListAvailable`

### Update the Az module

`Update-Module -Name Az`

## Virtual Machine

### Pricing calculator

[Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/)

### Templates

- VM can be deployed using the templates.
- Templates are two json file, one is template and other one is parameters

## App Service

### App Service Plan

- F1 - Shared infrastructure - Free
- D1 - Shared infrastructure - Paid
- B1 - 100 Total Azure Compute Units(ACU) - Paid
- B2 - 200 Total Azure Compute Units(ACU) - Paid
- B4 - 300 Total Azure Compute Units(ACU) - Paid

### Web App Service Resources

- Insights
- Web App

### Web Job

- Web job is the back ground task that could be associated with the WebApp
- We can have as many web jobs as we need

### Deployment slots

- We have deployment slot for staging, testing, dev etc
- These are deployed on to the same WebApp resource
- The deployment slots can also be swapped enabling us to easily switch product and staging environment
- In a single app service we can install multiple app based on the service plan. 10 for free, 100 for standard and unlimited for basic and up.
- Deployment slots are available in Standard, Premium and Isolated App Service plan. Each service plan supports limited number of slots. There is no additional charge for using deployment slots.
- Before swapping, make sure all instances are warmed up. This will ensure no downtime of the site
- If pre-swap validation is not need then the entire swap process could be automated by configuring auto swap

### Step during the swap process

> Target slot is the one in production and source slot is the on in stage

1. Settings like ConnectionString, authentication etc are applied from target slot to source slots

2. After settings are applied the source instances are restarted. If restart fails then the settings are reverted and swap process stops.

3. If local cache is enabled then trigger HTTP request to source slot to initialize the local cache

4. If auto swap is enable with custom warm up then trigger application initialization by making the request

5. After the source slots are warmed up the two slots are swapped.

6. The the source slot has the production. Apply setting and restart this slot.

### Monitoring and Diagnostic tools

- Alerts - Create rules to get notified on events
- Metrics - To monitor cpu, memory etc
- Diagnostics logs - Enable logs
- Log stream - Live application logs, this are the logs that go into filesystem. The filesystem logs are turned off after 24 hours.

### Powershell and CLI commands

- Create the resource group
- Create the service plan
- Create the WebApp
- Shortcut to create the webapp. This creates the resource group and plan. The --html is the type of app that it picks from current folder
  `az webapp up --location eastus --name [name] --html`

### Access the command line console app

- Using console we can cd into the web folder and check the files
- Kudu - This is utility similar to powershell

## Containers

- In project enable docker container
- Enabling would install the Windows subsystem for Linux(WSL)
- Register the azure container
- Publish the project to the container
- Enable the access in the registry
- Azure containers are very simple and doesn't scale well
- To deploy use create container instance and follow the prompts (Azure container Instance(ACI)

## Functions

- Supports .Net Core, Node.js, Python, Java, PowerShell Core, Custom Handler
- Custom Handler is for other language support
- Functions need the storage account
- Plan as an option of Serverless, Premium and App plan
- The functions app can contain multiple functions
- Functions have trigger. Like Http, timer etc
- Functions are short lived, simple and trigger by a event.
- Regardless of the function app timeout setting, 230 seconds is the maximum amount of time that an HTTP triggered function can take to respond to a request. This is because of the default idle timeout of Azure Load Balancer. For longer processing times, consider using the Durable Functions async pattern or defer the actual work and return an immediate response.

### Function integration

- Trigger
- Input binding
- Other functions
- Output binding

### Durable Functions

- These are functions that could take long time to run
- These functions can call other functions
- These can self suspend
- There is orchestrator, state and other functions which does unit work

### Durable Function Patterns

- Chaining pattern - ⚡F1 -> ⚡F2 -> ⚡F3
- Fanout pattern - function spans multiple parallel functions and waits for them to complete
- Asynchronous function
- Monitor pattern
- Human interaction pattern

### Durable Function Creation

- Create a function app
- Go App Service Editor
  - Create a package.json file
- Go to console
  - Install durable-functions package `npm install durable-functions`
- Create durable function using the template
  - This durable function is http trigger
  - This durable function starts the orchestrator function
- Create the orchestrator function
  - This orchestrator function calls multiple activity functions
- Create the activity functions
- For delays use moment library

### Using other tools

- Functions could be created from Azure CLI, Visual Studio and VS Code.
- Custom languages also could be used. Refer help docs.

### Microsoft Learn

- [Go to microsoft learn to create function apps](https://docs.microsoft.com/en-us/learn/modules/create-serverless-logic-with-azure-functions/3-create-an-azure-functions-app-in-the-azure-portal?pivots=javascript)
- [Code Samples](https://github.com/Azure-Samples/durablefunctions-apiscraping-dotnet)

## Storage Accounts

- It cost 2c/GB/Month i.e Quater/GB/year.
- Blog storage type is only for blobs and some security over it.
- V2 is some latest storage type
- Types of storage: Blog, Files, Table, Queue, Data Lake

### Redundancy

1. Locally redundant - 3 copies in the same zone and data center
2. Zone redundant - 3 copies in different zones
3. Geo Redundant - 6 copies, 3 local and 3 regional
4. Geo Zone -
5. Read access - read and write access url

### Network

- Public endpoint - All network
- Public endpoint - Selected networks
- Private endpoint

### Advanced options

- Secure transfer required
- Azure files - Largest is 100TB
- Soft delete - It will be saved for 7 days by default
- Data Lake Storage Gen2 (ADLS Gens) - More like NFS file system

### Blob Container

- Create the storage account.
- Create the container.

### Storage Access

- Using Access Keys - This gives complete access to the account.
- Shared Access Signature - Controlled way of giving access.

## CosmosDB

- No SQL database
- 10 ms latency guaranteed
- [CosmosDB Samples](https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app)
- Styles for CosmosDB
  1. Core (SQL)
  2. Azure Cosmos DB API for MongoDB
  3. Cassandra
  4. Azure Table
  5. Gremlin(Graph)

### Create CosmosDB Account

#### Main details

- Enter the resource group
- Account Name
- Location
- Capacity mode - Provisioned throughput or Serverless
  - Serverless is bill according to number of execution
  - Provisioned is traditional type
- Tier
  - Free tier has 1000 Request Units per sec RU/s and 25 GB storage

#### Global Distribution

- Geo Redundancy - Price is doubled
- Multi-region Writes - Price is doubled

#### Networking

- All networks
- Public endpoint (selected networks)
- Private endpoint

#### Backup Policy

- Periodic
  - Every 4 hours and retains 8 hours, i.e 2 copies of retention
- Continuous (Preview)
- Backup redundancy
  - Geo or Local

#### Encryption

- Its encrypted by default.
- Encryption key can be managed by Azure or customer managed in azure key vault.

### Option and Settings

- Access Control(IAM) - For role based control.
- Quick Start - Select the programming language.
- Replicate data globally - Replicate the data across glob.
- Keys - Security
  - Contains endpoint
  - Primary key and Secondary key
  - Primary and Secondary connection strings
  - Same set of keys for readonly replicated data

### Containers (DB)

- Enter name of the db
- Option to Share throughput across the containers
- Auto scale or Manual scale
- Database Max RU/s
  - If Auto scale then select minimum
  - If manual then add script to scale
  - 1 RU/s means 1KB read per sec
- Container id -Enter name of the container(table)
- Partition key
  - Its the logical grouping of the data
  - Make sure these groups are evenly distributed
  - If not then there could be performance problems
- Adding items to the container is simple
  - Use json object format to add the records

### Metrics

- Metrics giving the RU/s data

### Develop for CosmosDB

- Could be coded in .Net, Node Js, Python etc

### Default consistency

- Session - Default - Depends on the client read and availability. For same client it may be consistent but for different client it may be lagging.
- Strong - It is replicated instantaneously.
- Bounded Staleness - Depends on the time lap specified.
- Consistent Prefix - One region is guaranteed.
- Eventual - This is the weakest and order is not guaranteed. This suited for databases like tweets, likes etc.

### CosmosDBChangeFeed Functions

- Function Name
- CosmosDB Account Connection
- Database Name
- Collection Name
- Leases - This is to reserve that the function is executing on this DB change.

## Relational Database Storage

- SQL Server Database VM
- Azure SQL Server Database
- Azure SQL Server Managed Database - Azure does the database management and tuning
- SQL Data Warehouse - For reporting
- [SQL Database Samples](https://github.com/Azure-Samples/dotnet-sqldb-tutorial)

### Database Creation

- Subscrition
- Resource group name
- Database name
- Database Server
- Elastic pool - Share the database instance load
- Compute and Storage
  - Standard
    - DTU - this is relative unit
    - Storage size
  - Premium
    - Provisioned
      - Specify number of V cores
      - Data max size
    - Serverless
      - Max and Min V cores
      - Auto scales
- Network
  - No access
  - Public endpoint
  - Private endpoint
- Addition settings
  - Empty database
  - Backup
  - Sample
  - Data Collation
  - Advanced security

### Connecting to the database

- Goto overview->Fire wall->Add IP address
- Connect using the web interface
- Connect using SQL Server Management Studio

### Geo replication

- Can create the readonly replication of the database

## Blob Container Access

- Blob is just a container we through files into.
- Create a blob container
  - Name
  - Access level
    - Private (no anonymous access)
    - Blob ( anonymous read level access for blobs only)
    - Container ( anonymous read level access for container and blobs)
- While upload file we can also create a folder but these are virtual folders.
- [Storage Samples](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started)
- [Azure Quick Starts](https://github.com/AzureADQuickStarts)

### Az copy

- This is utility to copy files on to blobs
- Download and install
- Include azcopy into your path
- Open powershell and login to azure using `az login`
- Run the command to copy
  - `AzCopy /Source:http:[url] /Dest:http:[url] /SourceKey:[key] /DestKey:[key]`
- It copies on server side
- `/SyncCopy` downloads into memory and then copies to destination

### Leases

- You can acquire a Lease to lock the file
- To write, delete you need to include the lease id
- You also delete by breaking the lease

### Access tiers

- By default the files are on Hot tier
- If you are not going to access the files regularly then can the tier to cold or archive
- Cool tier is 30 days lease
- Archive tier is 180 days and takes time to restore

## Azure Active Directory

- Azure Active Directory is not same as Active directory
- You can use Azure AD Connect to sync up the active directory users to azure active directory
- App registrations can be used to make the application single sign on
- [Azure Active Directory Samples](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2)

### Create Active Directory

- Search for Azure active directory in the resource to create
  - Organization name
  - Domain name
  - Country or region
- You can switch the Azure Action director
- When switch its is new tenant and you don't see the previous items

### Application Registration

- Go to Azure portal
- Register an application
  - Who can use this application or access this API
    - Accounts in this organizational directory only (Single tenant)
    - Accounts in any organizational directory only (Multi tenant)
    - Accounts in any organizational directory only (Multi tenant) and personal microsoft account (Skype, Xbox)
    - Personal accounts only
- Redirect URI (Optional)

## Azure Access Control

- Role Based Access Control (RBAC)

  - Owner - Full access
  - Contributor - Access less than owner and more than Reader
  - Reader - Low level access

- Access Keys

  - Full access to the storage account
  - Two keys are available
  - You can generate the keys to invalidate

- Shared Access Signature (SAS)
  - Has more granular security options
  - Can be invalidated by Access key

## Data Security and Encryption

- All data is encrypted by default
- The encrption keys are managed by azure but it gives an option to use your own encryption keys
- Security at transit
  - You would enable `Secure transfer required` option in the configuration. This enables https and ssl.

### Data Encryption and SQL database

- Database, backups, logs etc are encrypted by default at database server level
- The encryption keys are managed by azure. In case you want to use your own key then go to `Security->Transparent data encryption` and check `Use your own key`
- Encryption can be disabled at database level but not database server level.
- The master database is can't and not be encrypted.

### Azure Key Vault

- Create a key vault
  - Standard tier is 4c per month
  - Premiums is 1.32 dollars per month. This uses the Hardware security solution (HSM)
- Network access
  - Allow all networks
  - Allow selected networks
- You can store:
  - Keys - Public and private key pair
  - Secrets
  - Certificates
- [Sample](https://github.com/Azure-Samples/key-vault-dotnet-manage-key-vaults)

### Configure TLS mutual authetication

- For ASP.Net applications the certificate is send through HttpRequest.ClientCertificate property.
- For other types of apps like PHP, Node.js etc the certificate is available through X-ARR-ClientCert request header.

## Scaling Apps and Services

- Free Tier doesn't have the Scale up and Scale out option.
- Basic plan has Scale up and Scale out. But they are manual.
- Production plan has Scale up and Scale out and also auto options.
- Scale up - Changing the tier to increase or decrease the computing power.
- Scale out - To increase or decrease the Azure Compute Units (ACU).
- [Sample](https://github.com/Azure-Samples/app-service-dotnet-scale-web-apps)

### Auto Scaling

- Scaling apps base on rules
- Scaling VM instances
- Scaling Single VM by resizing

### Handling Transient Faults

- Failure handling

## Caching and Content Delivery Networks(CDNs)

### Redis Cache

- StackExchange.Redis is the standard package for Redis .Net
- Backend type cache

### CDN

- CDN runs on global scale so you won't select the region while creating the CDN

### Create CDN

- Profile
  - Three tiers - Microsoft, Verizon, Akamai
- End point
  - Specify the orgin and endpoint

## Monitoring

- Enable the Diagnostic settings
- Selection the types of logs from different settings
  - Performance counters
  - Logs
  - Crash dumps
  - Sinks
  - Agent
- Also go to logs and enable the logs to a workspace

### Azure Monitor

- Select the VM to see the graphs etc

## Azure Services

### Logic Apps

- Create logic App
- Go to logic app designer
- You can select the existing template or create your own trigger
- Example you can select the Http request to trigger
- And select the corresponding response

### Azure Search

- Create Azure Search
- Pricing Tier
  - Free
    - 3 Indexes
    - 50 MB
    - Shared Resource
  - Basic
    - 15 Indexes
    - 2 GB
    - 3 Search Units
    - 3 Replicas
    - Dedicated Resource
  - Standard
    - 50 Indexes
    - 25 GB/Partition
    - Dedicated Resource
    - 36 search units
    - 12 replicas
    - 12 partitions

### API Management

- Create API management service

### Events

- Event Grid - Used to hook up with the events raised in the Azure and react
- Event Hub - Hook to the events occurring outside of the Azure
- IoT Hub - Events occurring in IoT devices

## Application Queues

### Azure Storage Queues

- Create a Queue
- Queue gets a url
- Application can add messages into queue
- The message can be in plain text or json format
- Other application can read and dequeues
  - Peek message - Get the message without removing
  - Get Message - Gets the message and you need to delete it within 30 seconds (You can change this time)

### Service Bus Queue

- Create Service Bus
  - Subscription
  - Resource group
  - Namespace name
  - Pricing tier
    - Premium tier has topics and the message size is large

## Virtual Network (VNet)

- Premium plan support serverless scale while supporting network
