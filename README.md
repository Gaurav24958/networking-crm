# Networking CRM

A lightweight, modern, single-page CRM tool designed to log and manage recruiter and hiring manager information. This application connects a sleek frontend interface directly to Google Sheets using Google Apps Script as a serverless backend.

## 🚀 Features

- **Modern UI**: Sleek dark mode design with responsive styling using native CSS variables.
- **Serverless Backend**: Directly uploads data to a Google Sheet without any intermediate servers or databases.
- **Actionable Tracking**: Log essential recruiter details, where you met them, their LinkedIn URL, and follow-up status.
- **Fast and Portable**: Built using pure HTML, CSS, and Vanilla JS in a single file. No build steps, bundlers, or external dependencies required.

---

## 🛠️ Tech Stack

- **Frontend**: HTML5, Vanilla CSS3 (custom properties/variables, flexbox, transitions), and Modern JavaScript (async/fetch API).
- **Backend/Database**: Google Sheets (data storage) & Google Apps Script (REST API handler).

---

## 📋 Contact Schema

The CRM logs the following fields for every connection:

| Field Name | Type | Description | Required? |
| :--- | :--- | :--- | :--- |
| **Name** | Text | The contact's first and last name | **Yes** |
| **Company** | Text | The company where they work | No |
| **Role** | Dropdown | Contact's role selection: `Manager` or `Recruiter` | **Yes** |
| **Where Met** | Text | Context of where you met (e.g., `LinkedIn`, `TCD Alumni Meetup`) | No |
| **LinkedIn URL** | URL | Link to their LinkedIn profile page | No |
| **Contact Info** | Text | Email address or phone number | No |
| **Notes** | Textarea | General context, discussion details, or specific actions | No |
| **Follow-up Status** | Dropdown | `Not Started`, `Message Sent`, `Responded`, `Meeting Scheduled` | No |

---

## ⚙️ Backend Setup Guide (Google Sheets & Apps Script)

Follow these steps to configure your own Google Sheets backend:

### Step 1: Create your Google Sheet
1. Open Google Sheets and create a new spreadsheet.
2. Set up your column headers in the first row. The columns should match the form keys exactly (case-sensitive):
   - `Timestamp`
   - `Name`
   - `Company`
   - `Role`
   - `Where Met`
   - `LinkedIn URL`
   - `Contact Info`
   - `Notes`
   - `Follow-up Status`

### Step 2: Write the Apps Script
1. In the spreadsheet menu, go to **Extensions** -> **Apps Script**.
2. Replace all code in the script editor with the following implementation:

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    // Columns order in the sheet
    const headers = [
      "Timestamp",
      "Name",
      "Company",
      "Role",
      "Where Met",
      "LinkedIn URL",
      "Contact Info",
      "Notes",
      "Follow-up Status"
    ];
    
    // Build row values
    const row = headers.map(header => {
      if (header === "Timestamp") {
        return new Date();
      }
      return data[header] || "";
    });
    
    // Append to sheet
    sheet.appendRow(row);
    
    return ContentService.createTextOutput(JSON.stringify({ status: "success" }))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ status: "error", message: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// Optional: Enable CORS (for preflight requests if needed)
function doOptions(e) {
  return ContentService.createTextOutput("")
    .setMimeType(ContentService.MimeType.TEXT);
}
```

### Step 3: Deploy the Script as a Web App
1. In the Apps Script editor, click the **Deploy** button in the top right and select **New deployment**.
2. Click the gear icon next to "Select type" and choose **Web app**.
3. Fill in the deployment details:
   - **Description**: Networking CRM API
   - **Execute as**: `Me (your-email@gmail.com)`
   - **Who has access**: `Anyone` *(Crucial: This allows the HTML page to submit data to the endpoint)*.
4. Click **Deploy**.
5. Copy the generated **Web App URL** (it ends in `/exec`).

### Step 4: Configure the HTML File
1. Open the [Networking_CRM.html](file:///Users/gaurav/Desktop/Networking_crm/Networking_CRM.html) file.
2. Locate the line with the `SCRIPT_URL` constant:
   ```javascript
   const SCRIPT_URL = "YOUR_DEPLOYED_WEB_APP_URL_HERE";
   ```
3. Replace the placeholder URL with your copied **Web App URL**.
4. Save the file.

---

## 🏃 Running the Application

Since the tool is a completely self-contained static HTML page, you can open and run it locally:

1. Double-click the [Networking_CRM.html](file:///Users/gaurav/Desktop/Networking_crm/Networking_CRM.html) file to open it directly in any modern browser.
2. Fill out the form fields and click **Save Contact**.
3. Verify that the contact appears in your Google Sheet immediately.

### 🌐 Hosting / Publishing (Optional)
If you want to access this CRM from your phone or outside your local machine, you can host the static HTML page for free:
- **GitHub Pages**: Create a repository, upload `index.html` (rename `Networking_CRM.html` to `index.html`), and enable Pages.
- **Vercel / Netlify**: Drag-and-drop the directory to publish in seconds.
