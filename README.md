# Pre-Defined List of Texts Display Before Redirecting to a Link

A **very simple** client-side JavaScript application that fetches data from a Google Spreadsheet and displays them to the user, in order to be copied. After copying a chosen text, the user can be directed to a **pre-defined link** for quick pasting. This approach uses JSONP to bypass CORS restrictions and requires a valid token passed via the URL.

---

## Table of Contents

1. [Overview](#overview)  
2. [Features](#features)  
3. [How It Works](#how-it-works)  
4. [Usage](#usage)  
5. [Project Structure](#project-structure)  
6. [Deployment](#deployment)  
7. [Best Practices](#best-practices)  
8. [License](#license)

---

## Overview

This project was created to streamline the process of:  
1. Fetching a set of **pre-defined texts** from a **Google Spreadsheet**.  
2. Displaying these texts in an HTML table where they can be quickly copied.  
3. Redirecting the user to a **pre-defined link** for easy pasting of the copied text.  

All logic is contained within a single HTML file and leverages native browser functions. The application does not rely on any server-side code, making it suitable for **static hosting** services such as **Amazon S3** and **CloudFront**.

---

## Features

- **Google Spreadsheet Integration**: Fetch data using JSONP, avoiding complex backend servers or APIs.  
- **Token-Based Access**: Dynamically filter spreadsheet rows via a `token` query parameter.  
- **Instant Copy**: Copy any text to the clipboard with a single click.  
- **Automatic Redirect**: Optionally lead the user to the specified **pre-defined link**.  
- **Error Handling**: Displays error messages if the `token` is missing or invalid.  

---

## How It Works

1. **Token Query Parameter**  
   - The application expects a query parameter like `?token=XYZ`.  
   - If no `token` is provided, an error message is shown.

2. **Google Spreadsheet**  
   - A call is made to a published Google Spreadsheet using JSONP.  
   - The app looks for a row where Column C (index `2`) matches the provided token.  
   - **Column A (index `0`)**: The **pre-defined link** (destination URL).  
   - **Column C (index `2`)**: The token used for matching.  
   - **Columns D onward (indexes `3, 4, 5...`)**: The **pre-defined texts** to display.  

3. **Rendering**  
   - If a matching row is found, the **pre-defined link** is set on the “Ir para o Link” (Go to Link) button.  
   - The **pre-defined texts** (Columns D onward) populate the table. Each row has a **Copiar** button.  

4. **Copy & Redirect**  
   - Clicking **Copiar** copies the selected text to the clipboard.  
   - The “Ir para o Link” button opens the destination URL in a new tab.

---

## Usage

1. **Open the Application**  
   - Host the `index.html` file on your preferred static hosting service.  
   - Append a `token` query parameter, for example:  
     ```
     https://yourdomain.com/?token=XYZ
     ```

2. **Provide a Valid Spreadsheet**  
   - In the code, set:
     ```js
     const SPREADSHEET_ID = 'FILL_WITH_YOUR_SPREADSHEET_ID';
     const SHEET_ID = 'FILL_WITH_YOUR_SHEET_ID';
     ```
   - Ensure your Google Spreadsheet is publicly accessible (published) and has a sheet named as configured in the `SHEET_ID` variable.
   - The row matching your `token` must include:
     - **Pre-defined link** in Column A (index `0`).  
     - **Token** in Column C (index `2`).  
     - **Pre-defined texts** starting from Column D (index `3`) onward.  

3. **Copy a Text**  
   - A table of **pre-defined texts** will be displayed. Click **Copiar** to copy the desired text.  

4. **Redirect to the Link**  
   - The “Ir para o Link” button opens the **pre-defined link** in a new tab. Paste your copied text wherever it’s needed.

---

## Project Structure

```plaintext
.
├── index.html   // Core HTML, CSS, and JavaScript in one file
└── README.md    // Project instructions and details

-  `index.html` contains:
    - **Basic styling** (in the `<style>` tag).
    - **A table** for displaying fetched comments.
    - **Client-side logic** in a `<script>` tag that:
        - Parses the `token` from the URL.
        - Fetches data from the Google Spreadsheet.
        - Filters the row by matching the token.
        - Displays the pre-defined texts and sets the pre-defined link.
```
---

## Deployment

This project is best deployed as a **static website**.  
**Amazon S3 + CloudFront** is recommended to resolve potential CORS issues, especially on mobile browsers.

### Steps to Deploy

1. **Create S3 Bucket**  
   - Make sure to enable static website hosting.

2. **Upload `index.html`**  
   - Upload the file directly.

3. **Create CloudFront Distribution**  
   - Point the distribution to the S3 bucket.  
   - Configure behaviors, CORS, and any custom headers as needed.

4. **Test**  
   - Access the CloudFront domain (or your custom domain) with `?token=XYZ` appended.

---

## Best Practices

### Secure Your Spreadsheet
- Only publish minimal data required for your application.  
- Consider restricting who can find or use your `token` parameter.

### Error Handling
- If the token is invalid or missing, the script already displays a user-friendly message.

### CORS
- Using CloudFront often resolves issues that might occur with S3 alone, especially on mobile.

### Version Control
- Tag your versions or commits so you can revert changes easily if needed.

---

## License

This project is open-sourced under the [MIT license](LICENSE). Feel free to use and adapt it as needed.

---

**Enjoy using or modifying this simple JSONP-based application! If you have suggestions or encounter issues, open an issue or submit a pull request.**