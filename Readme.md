Do not download the files. They are there for a forensic expert to access.

File shown to be malicious via https://www.virustotal.com/gui/file/ae50d9e1ad54fc3a876620f718a2c54112cf4b693496665766e14eb8d1e56cea/detection 

2 files are tainted:
ecommerce.js
safe-browsing.js

Analysis of both files as follows:

Analysis of Data Upload and Storage Based on ecommerce.js and safe-browsing.js

By combining the analysis of both ecommerce.js (which tracks e-commerce activity) and safe-browsing.js (which handles safety checks on URLs), we can determine what data is being uploaded or stored and where.

1. Data That is Uploaded
Both scripts interact with external servers by uploading various types of data:

1.1 ecommerce.js Uploads:
What is Uploaded?
User browsing activity (web navigation events, coupon searches, etc.).
Product and price details from e-commerce sites.
Authentication tokens (e.g., fetchSecurityToken sends data for authentication).
Tracking data and analytics (e.g., track function in TrackClient sends logs).
Error logs and debugging data (e.g., ErrorTraceClient reports issues).

Where is it Uploaded?
The script makes API requests to remote servers (most likely owned by the extension provider).
The exact URLs are not visible in the extracted portions, but it is clear that requests go to external APIs.

1.2 safe-browsing.js Uploads:
What is Uploaded?
URL safety checks: The script sends URLs the user visits to an external API (checkSafety function).
Navigation details: Includes timestamp, tab ID, and browser metadata.
Device and browser info: OS, browser version, and referrer information.
HTTP request and response headers: Sent to assess whether a site is safe.
Error logs: If URL safety checks fail, logs are uploaded.

Where is it Uploaded?
The script uses an endpoint such as:
"/secure/urls/checkSafety/basic"
"/secure/urls/checkSafety"

The domain of the API is likely specified in this.options.apiUrl.
Data is sent via fetch() requests with compressed JSON payloads.

2. Data That is Stored
Both scripts store data, but none of it appears to be stored in regular files on the user's laptop. Instead, it is stored within the browser.

2.1 ecommerce.js Storage:
Stored Where?
chrome.storage.local (persistent storage across browser restarts).
chrome.storage.session (temporary storage).

What is Stored?
Security tokens (E_COMMERCE_SECURITY_TOKEN).
Configuration settings (e.g., which websites are enabled).
Possibly cached coupons and pricing data.

2.2 safe-browsing.js Storage:
Stored Where?
chrome.storage.local (long-term storage).
chrome.storage.session (short-term session storage).
BgNavStorage and FgNavStorage classes use:
Compressed JSON stored in chrome.storage.local.

What is Stored?
Cached safety check results for previously visited sites.
Browser navigation data (tab info, referrer URLs).
Response details (redirects, HTTP headers, and errors).

3. Is Any Data Saved as a File on the Laptop?
No, neither script writes data to actual files on the user's filesystem.
The data is stored inside browser storage (chrome.storage.local, chrome.storage.session), which is not directly accessible as a file in the usual file system.

Summary from DS
Data Transferred to External Servers:
Tracking data (e.g., product page views, coupon usage) is sent to https://api.tnagofsg.com/rest/v1.
Coupon and deal data is fetched from https://api.tnagofsg.com/rest/v1.
Price trend data is fetched from https://api.tnagofsg.com/rest/v2.
Error traces are sent to https://api.tnagofsg.com/rest/v1.
Security tokens are fetched from https://api.tnagofsg.com/rest/v1.

Data Downloaded and Storage Locations:
Coupon and deal data is stored in the browser's sessionStorage.
Price trend data is stored in memory and is not persisted to disk.
Security tokens are stored in the browser's localStorage.
Error traces and user tracking data are not stored locally; they are sent to the server.

This extension primarily interacts with external servers to fetch and send data, and it uses the browser's storage mechanisms (sessionStorage and localStorage) to store certain types of data temporarily or persistently.
