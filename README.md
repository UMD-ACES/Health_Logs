## This guide details how to set up logging from your host to a spreadsheet in Google Drive.

1. Go to the [Google APIs Console](https://console.developers.google.com/)
2. Click the dropdown menu in the top left corner to select a project.
3. In the top right corner of the popup, click _Create a new project_.
4. Click _Enable API_. Search for and enable the Google Drive and Google Sheets APIs.
5. Create credentials for a _Web Server_ to access _Application Data_.
6. Name the service account and grant it a _Project Role_ of _Editor_.
7. Download the JSON file.
8. Copy the JSON to your code directory and rename it to `health_log_creds.json`.
9. Find the `client_email` variable inside `health_log_creds.json`. 
10. Back in your spreadsheet, click the Share button in the top right, and paste the client email into the People field to give it edit rights. Hit Send.
11. Download `pip3` for Python
```
sudo apt update
sudo apt install python3-pip
```
12. Download the `gspread` and `oauth2client` Python libraries
```
pip3 install gspread oauth2client
```
13. Copy the following code into your Python health script.
```
import gspread
from google.oauth2.service_account import Credentials
# use creds to create a client to interact with the Google Drive and Google Sheets API
scope = ["https://spreadsheets.google.com/feeds",'https://www.googleapis.com/auth/spreadsheets',"https://www.googleapis.com/auth/drive.file","https://www.googleapis.com/auth/drive"]
creds = Credentials.from_service_account_file('health_log_creds.json', scope)
client = gspread.authorize(creds)

# Put the name of your spreadsheet here
sheet = client.open("<Spreadsheet Name>").sheet1

# Example of how to insert a row
row = ["I'm","inserting","a","row","into","a,","Spreadsheet","with","Python"]
index = 1
sheet.insert_row(row, index)

# Example of how to delete a row. This deletes the first row.
sheet.delete_row(1)

# Example of how to update a single cell
sheet.update_cell(1, 1, "Update top left cell")

# How to get the number of rows in the spreadsheet
sheet.row_count
```
