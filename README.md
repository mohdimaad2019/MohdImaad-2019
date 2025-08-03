📄 SAP B1 to DMS - Sales Invoice Integration
This integration retrieves AR Invoice data from SAP Business One, converts it to JSON format, and posts it to the DMS (Document Management System). On successful post, it returns the response and updates a UDF (User Defined Field) in SAP B1 to track sync status.

🔗 API Details
📥 SAP B1 Source
Object Type: AR Invoice (OINV)

Method: GET (From SAP B1 SDK/DIAPI/Service Layer)

Example Fields Retrieved:

DocEntry

CardCode

CardName

DocDate

DocTotal

Comments

DocumentLines[]

📤 DMS Target
Endpoint URL: https://your-dms-url.com/api/invoices

Method: POST

Content-Type: application/json

📦 Sample JSON Body (AR Invoice to DMS)
json
Copy
Edit
{
  "InvoiceNumber": "12345",
  "CustomerCode": "C0001",
  "CustomerName": "ABC Traders",
  "InvoiceDate": "2025-08-01",
  "TotalAmount": 15000.00,
  "Remarks": "Payment due in 30 days",
  "LineItems": [
    {
      "ItemCode": "I1001",
      "Description": "Product 1",
      "Quantity": 2,
      "UnitPrice": 5000.00,
      "LineTotal": 10000.00
    },
    {
      "ItemCode": "I1002",
      "Description": "Product 2",
      "Quantity": 1,
      "UnitPrice": 5000.00,
      "LineTotal": 5000.00
    }
  ]
}
🔁 Process Flow
Fetch Invoice from SAP B1 using DIAPI/Service Layer based on filters (e.g., U_DMS_Status = 'Pending').

Convert SAP B1 AR Invoice structure to required JSON format.

POST the JSON to DMS API endpoint.

Receive Response from DMS (e.g., success, document ID, etc.).

Update SAP B1 UDF U_DMS_Status = 'Synced' and optionally save U_DMS_DocID.

🧪 Sample Success Response from DMS
json
Copy
Edit
{
  "status": "success",
  "documentId": "DMS-2025-INV-1234"
}
📝 SAP B1 UDF Fields Updated
Field Name	Type	Description
U_DMS_Status	String	'Synced' / 'Failed'
U_DMS_DocID	String	DMS Document Reference ID

⚙️ Configuration
Make sure your integration service or script has:

SAP B1 connection (DIAPI or Service Layer)

Error handling for network/API issues

Logging for success/failure

Configurable DMS URL and credentials

📁 File Structure Example
arduino
Copy
Edit
.
├── src/
│   ├── sap_invoice_fetch.cs
│   ├── dms_post.cs
│   └── udf_update.cs
├── README.md
├── config.json
└── logs/
