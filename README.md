## 📧 Montana House Email Automation - HB 176

### 🛠 Preface: Necessary Tools  

To ensure you have everything needed to run this script, install the following tools:  

🔹 **[Git Bash (for Windows users)](https://www.atlassian.com/git/tutorials/git-bash#:~:text=Git%20Bash%20comes%20included%20as,open%20to%20execute%20Git%20Bash.)**  
🔹 **[Git for Windows (if not installed)](https://gitforwindows.org/)**  
🔹 **[Python (required to run the script)](https://www.python.org/downloads/)**  

Before proceeding, **make sure these are installed**.

---

## 📌 Introduction  

This project **automates emailing Montana state representatives** to express concerns about **HB 176**. It leverages Google APIs to send emails via Gmail and logs interactions in Google Sheets.  

### ✅ Features  

- ✉️ **Automatically sends personalized emails to senators**
- 📝 **Uses pre-defined email templates** to ensure messages are concise and effective
- 📊 **Logs sent emails in a Google Sheet** for tracking
- 🔄 **Supports both "Test Mode" and live sending**

---

## 🚀 Step-by-Step Setup  

### 1️⃣ Clone the Repository  

Open a **terminal (Git Bash/Command Prompt)** and run:  

```sh
git clone https://github.com/Eldres/MT-HB176-Auto-Email-Script.git
cd MT-HB176-Auto-Email-Script
```

---

### 2️⃣ Create a Google Cloud Project  

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. **Click "New Project"**
3. Give it a name (e.g., **MT House Email Automation**) and create it

![Create New Project](readme_pics/NewProject.png)

---

### 3️⃣ Enable Required APIs  

You must enable the **Gmail API** and **Google Sheets API** for this script to work.  

1. Open the **Google Cloud Console**  
2. Navigate to **APIs & Services** > **Library**  
3. Search for **Gmail API** and **Google Sheets API**  
4. **Enable both**  

![Enable APIs](readme_pics/enable_api_start.png)

---

### 4️⃣ Create OAuth Credentials  

1. Navigate to **APIs & Services** > **Credentials**  
2. Click **"Create Credentials"** and select **"OAuth Client ID"**  
3. Under **Application Type**, choose **Desktop App**  
4. Click **"Create"**, then **download the JSON file**  
5. Save it in the project directory as:  
   ```plaintext
   oauth.json
   ```

![Create OAuth Client](readme_pics/create_oauth_1.png)

---

### 5️⃣ Create a Service Account  

1. Go to **APIs & Services** > **Credentials**  
2. Click **"Create Credentials"** and select **"Service Account"**  
3. Enter a **name & description**, then **click Create**  
4. Under **Keys**, click **"Add Key"** > **JSON**, then **download it**  
5. Save it in the project directory as:  
   ```plaintext
   service_account.json
   ```

![Create Service Account](readme_pics/create_service_acct_1.png)

---

### 6️⃣ Grant Access to Google Sheets  

1. **Open your Google Sheet**
2. Click **Share** (top-right)
3. Add the **Service Account Email** with **Editor Access**
4. Click **Done**

![Share Google Sheet](readme_pics/sheets_service_acct_share.png)

---

### 7️⃣ Install Dependencies  

Run the following in **Git Bash/Command Prompt** inside the project folder:

```sh
pip install -r requirements.txt
```

---

### 8️⃣ Set Up Environment Variables

To properly configure the script, you need to create a `.env` file and include the necessary credentials.

#### 📌 How to Find Your Google Sheets ID:
1. Open your Google Sheet in your browser.
2. Look at the URL, which will be in this format:
   ```plaintext
   https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit#gid=0
   ```
3. Copy the long string between `/d/` and `/edit` – this is your `SHEET_ID`.

Example:
```
https://docs.google.com/spreadsheets/d/1A2B3C4D5E6F7G8H9IJKLMNOPQ/edit#gid=0
```
The `SHEET_ID` is:
```
1A2B3C4D5E6F7G8H9IJKLMNOPQ
```  

In the project folder, create a **.env file** and add:

```plaintext
SHEET_ID=your_google_sheet_id
TEST_MODE=True
GMAIL_CLIENT_SECRET=oauth.json
SERVICE_ACCOUNT_FILE=service_account.json
```

---

### 9️⃣ Run the Script  

🛠 **Test Mode (safe mode - does not send real emails):**  

```sh
python send_emails.py
```

🚀 **Live Mode (sends real emails):**  

```sh
TEST_MODE=False python send_emails.py
```

---

### 🔄 1️⃣0️⃣ Automate Script Execution (Task Scheduler for Windows & Mac)  

To ensure the script runs automatically every day, follow the instructions below:  

#### 🖥️ Windows: Using Task Scheduler  

1. Open **Task Scheduler** by searching for it in the Windows Start Menu.  
2. Click **Create Basic Task…** on the right-hand panel.  
3. Name it **MT House Email Automation** and click **Next**.  
4. Under **Trigger**, select **Daily** and set a time you want the emails to be sent. Click **Next**.  
5. Under **Action**, select **Start a Program**, then click **Next**.  
6. In **Program/Script**, browse and select **python.exe** (found in your Python installation directory).  
7. In **Add arguments**, enter:  
   ```
   C:\path\to\your\project\send_emails.py
   ```
   Replace `C:\path\to\your\project\send_emails.py` with the actual path to your script.  
8. Click **Finish**. Your script will now run automatically at the scheduled time.  

📝 *To edit or remove the task, go to Task Scheduler > Task Scheduler Library > Find your task and modify as needed.*  

#### 🍏 Mac: Using `launchd` (Launch Agents)  

1. Open **Terminal** and create a new plist file:  
   ```sh
   nano ~/Library/LaunchAgents/com.House.emailautomation.plist
   ```  
2. Add the following XML content:  

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
       <dict>
           <key>Label</key>
           <string>com.House.emailautomation</string>
           <key>ProgramArguments</key>
           <array>
               <string>/usr/bin/python3</string>
               <string>/Users/YOUR_USERNAME/path/to/project/send_emails.py</string>
           </array>
           <key>StartInterval</key>
           <integer>86400</integer> <!-- Runs every 24 hours -->
           <key>RunAtLoad</key>
           <true/>
       </dict>
   </plist>
   ```
   Replace `/Users/YOUR_USERNAME/path/to/project/send_emails.py` with your actual script path.  

3. Save the file (`CTRL + X`, then `Y` to confirm changes).  
4. Load the scheduled task into `launchd`:  
   ```sh
   launchctl load ~/Library/LaunchAgents/com.House.emailautomation.plist
   ```  
5. To verify, run:  
   ```sh
   launchctl list | grep emailautomation
   ```  
   If your job is listed, it is now running daily.  

🔄 *To remove the automation, run:*  
```sh
launchctl unload ~/Library/LaunchAgents/com.House.emailautomation.plist
```

---

## 📊 Logs & Tracking  

- Emails sent are **automatically logged** in your Google Sheet  
- If `TEST_MODE=True`, emails **are simulated and recorded but not sent**  

---

## 🔧 Troubleshooting  

### ❌ Authentication Failure  

✅ Ensure `oauth.json` and `service_account.json` are placed correctly  
✅ Ensure the **Service Account Email** has access to your Google Sheet  

### 📉 Google Sheets Logging Not Working  

✅ Verify that **Google Sheets API is enabled**  
✅ Double-check the **Sheet ID** in `.env`  

If issues persist, refer to [Google API Documentation](https://developers.google.com/).

---

## 🐜 License  

This project is **open-source under the MIT License**.  

For questions or contributions, **submit an issue or pull request**! 🚀  

---
