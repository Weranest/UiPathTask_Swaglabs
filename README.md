### Created by Eimantas Bernotavicius ###
### Documentation is included in the Documentation folder ###


### What it does ###
The purpose of this package is access the website "https://www.saucedemo.com", login using stored credentials, select a number of items to buy, checkout, log off and provide a report of items purchased.

### How to operate it ###
 1. Program uses Chrome, Excel, Word and Outlook as part of required operations, as such the machine the program is used on should have the listed software installed.
 2. Create a "Grupės.xlsx" file that contains one collumn named "Pavadinimas". Insert the search terms needed inside the collumn.
 3. Change the desired settings and variables inside the "Config.xlsx", found inside the Data folder, for the dollowing settings:

 	+ WorkFolderPath - Path to the work folder used to store files (the program will create a new folder inside the selected path and will not interact with any other file present in the path)
 	+ EmailTo - Email address that is to be used as the recipient of the zip files at the end of the process
 	+ SongsPerBand - Number of songs to be included in each search results final output
 	+ MasterfilePath - Path to the "Grupės.xlsx" file
	+ MaxVideoLength - REQUIRED format: hh:mm:ss ///// Maximum length of videos to be included in the final output. When parsed, videos with higher values than the one here will not be parsed
	+ MaxVideosParsed (not used currently) - Maximum videos parsed for each band. Higher numbers are possibly more prone to breaking and take longer, but are more likely to return the correct amount of videos requested


 4. Run through studio or publish as a package. Since the assets are modified directly through config, and not orchestrator, publishing will lock out the edditing of said assets.


### How It Works ###

1. **INITIALIZE PROCESS**
 + ./Framework/*InitiAllSettings* - Load configuration data from Config.xlsx file and from assets
 + ./Framework/*GetAppCredential* - Retrieve credentials from Orchestrator assets or local Windows Credential Manager
 + ./Framework/*InitiAllApplications* - Open and login to applications used throughout the process
 + ./Framework/*SetupWorkFolder* - Sets up an isolated work folder to handle the files ised in the process


2. **GET TRANSACTION DATA**
 + ./Framework/*GetTransactionData* - Fetches transactions from the queue which is loaded from the selected excel file in the configs

3. **PROCESS TRANSACTION**
 + *Process* - Process trasaction and invoke other workflows related to the process being automated 
 + ./Framework/*WordCreator* - Downloads the images from the input bands and builds both the word documents and the zip files
 + ./Framework/*BrowserParser* - Uses youtube to get the required information about videos that were displayed by the selected search terms

4. **END PROCESS**
 + ./Framework/*CloseAllApplications* - Logs out and closes applications used throughout the process
 + ./Framework/*SendEmail* - Sends the email with the requested zip folders to the specified email adress using Outlook


### Known issues ###

* Project currently only supports Chrome for browsing youtube
* MaxVideosParsed currently does nothing - youtube parsing breaks when loading more than roughly 20 ids, will be a future fix