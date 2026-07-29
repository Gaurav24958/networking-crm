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
