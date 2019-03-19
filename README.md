## This guide details how to set up logging from your host to a spreadsheet in Google Drive.

On the Proxmox host, begin by downloading the repository: 

	apt-get install git
	git clone https://github.com/UMD-ACES/Health_Logs

Now that you've downloaded the code, install the necessary dependencies:

	cd Health_Logs
	./setup.sh

Head to the <a href="https://console.developers.google.com/apis/library/sheets.googleapis.com?q=Sheets">Google Developers Console</a>. Click on "Select a Project" in the top right corner. Then select "New Project". Name the project anything (e.g. HACS Project Health Logs).

Wait for the project to load, and then next to "Google Drive API", make sure to click "Enable". If you are not directed to the Google Drive API, simply click this <a href="https://console.developers.google.com/apis/library/sheets.googleapis.com?q=Sheets">link</a>

- Click on one of the tabs on the left labeled "Credentials". 
- Click on “Create Credentials”. You will then skip this procedure by clicking on "service account" (second line)
- Click on "Create Service Account" 
  - Makeup a name for the service account display name
  - No need for a description
- Click on "Create"
- Simply click on "Continue" when it asks you for "Service account permissions"
- Under "Create Key", click on "Create Key". Select JSON and download the file.
- Click on "Done"

Now that you've downloaded the .json file, you need to move the file to your host. There are a number of ways to do this - either using scp or uploading the file to dropbox and using wget. If you have port forwarding enabled, you could also copy and paste. You will need the location of this file for later.

You're almost there! Open the .json file, and you'll see an email associated with the "client_email" field. Copy it and share ALL your google sheets (or just your folder) with that email.

![image](https://cloud.githubusercontent.com/assets/14065974/22453754/0ec0ccb8-e74f-11e6-8b5f-f841df75119d.png)


To run the script, use:

	log -k <yourJSONfile> -s <sheetid> -d <data>

Where you need to replace the things inside the angle brackets (and the angle brackets themselves!) with the following data:

1. The JSON file should be the absolute path to the file, like:
	/root/stuff/hacs.json

2. And your sheetid is the URL of the Google Sheet. This is the full url, including https:// and edit#gid=0.

3. data is comma-separated values for your data, for example, "Toby,George,Matt,Louis-Henri". If I wanted to update the sheet with all the TAs' names, I would use the following command:

	log -k /root/stuff/hacs.json -s https://docs.google.com/spreadsheets/d/1H6uXIIxRPm4aSsxijklrm0BSoowdGrPDT-xr219kH-I/edit#gid=0 -d "Toby,George,Matt,Louis-Henri"


You've finished!! Now you need to figure out how often you want to run the logging command and which logs you want to update.

If you want, feel free to play around with the -w and -d options. For help, type:
	log --help
