# azure_blob
BLOB_AZURE

Let’s go step by step to create the Automated Blob Storage Monitoring & Alerting project in Azure. This project will:

✅ Monitor Azure Blob Storage for new file uploads
✅ Check if a tag (e.g., “code=mvgr”) exists in the metadata
✅ Send an email alert if the tag is missing (using Azure Logic Apps)

Step 1: Set Up Azure Blob Storage

1️⃣ Go to Azure Portal
2️⃣ Search for Storage Accounts → Click Create
3️⃣ Enter the required details:
	•	Subscription: (Select your free-tier subscription)
	•	Resource Group: Create a new one, e.g., blob-alert-rg
	•	Storage Account Name: myblobmonitor
	•	Region: Select any nearby region
	•	Performance: Standard
	•	Replication: Locally Redundant Storage (LRS)
4️⃣ Click Review + Create → Then Create

Step 2: Enable Event Grid to Detect New Blob Uploads

1️⃣ Open your Storage Account (myblobmonitor)
2️⃣ Go to Events → Click + Event Subscription
3️⃣ Name: blob-upload-event
4️⃣ Event Type: Select Blob Created
5️⃣ Destination: Choose Azure Function (we will create it next)
6️⃣ Click Create

Step 3: Create an Azure Function (Python) to Check Metadata

1️⃣ Create the Function App

1️⃣ Search for Function Apps in Azure → Click Create
2️⃣ Enter the details:
	•	Subscription: (Same as Storage Account)
	•	Resource Group: blob-alert-rg
	•	Function App Name: blob-monitor-func
	•	Runtime Stack: Python 3.9
	•	Region: Same as Storage Account
3️⃣ Click Review + Create → Then Create

2️⃣ Deploy the Python Code in Azure Function

1️⃣ Go to your Function App → Click Functions
2️⃣ Click + Create → Choose Event Grid Trigger
3️⃣ Name it BlobCheckFunction

4️⃣ Click Code + Test → Copy & Paste this Python code:

Step 4: Create Azure Logic App for Email Notification

1️⃣ Go to Logic Apps → Click + Create
2️⃣ Enter the details:
	•	Resource Group: blob-alert-rg
	•	Name: blob-alert-logicapp
3️⃣ Click Review + Create → Then Create

1️⃣ Set Up the Workflow

1️⃣ Open Logic App Designer
2️⃣ Click Blank Logic App
3️⃣ Search for HTTP Trigger → Choose “When an HTTP request is received”
4️⃣ Click + New Step → Choose Outlook 365 / Gmail - Send Email

2️⃣ Configure Email Settings
	•	To: Your email
	•	Subject: Alert: Missing Tag in Blob Storage
	•	Body:
 A new blob {{blob_name}} was uploaded but is missing the required tag 'code=mvgr'.
 5️⃣ Click Save
6️⃣ Copy the Webhook URL → Paste it into the Python function (LOGIC_APP_WEBHOOK_URL variable)

Step 5: Test the Setup

1️⃣ Upload a file to your Storage Account (myblobmonitor)
2️⃣ If it has the tag “code=mvgr”, nothing happens
3️⃣ If it does NOT have the tag, you receive an email alert 🚀


What This Project Teaches You?

✅ Azure Blob Storage (File Uploads & Event Grid)
✅ Azure Functions (Python) (Event-Driven Processing)
✅ Azure Logic Apps (Automated Email Notifications)
✅ Cloud Automation & Monitoring
