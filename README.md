# Python-Group_Gmail_Email_Sender-with-Attachments
A simple Python script to send HTML emails with attachments via Gmail using SMTP. Supports multiple recipients, CC, and BCC. Uses Gmail App Passwords for secure authentication.

# Features:

**1. Send HTML emails with custom subject and body.**

**2. Attach files to emails.**

**3. Support for multiple recipients, CC, and BCC.**

**4. Uses Gmail SMTP with TLS.**

**5. Secure authentication using Gmail App Passwords (16-character password, works with 2-Step Verification enabled).**

**6. Works on Linux, Windows, and macOS with Python 3.x.**

**    **
## Step 1
First, we need to download the Authenticator app from the Google Play Store. For my script using smtplib and Gmail, you must enable 2-Step Verification and generate a 16-digit App Password.

**    **
![authnaticator app](https://github.com/user-attachments/assets/305a94c7-9f06-401c-9ef3-f7f2f5685b97)
**    **
## Step 2
**visit the link below to set up the Google Authenticator app and obtain a 16-digit app password:**
```
https://myaccount.google.com/u/1/apppasswords?pli=1&rapt=AEjHL4MMXFWfbgWWYDe6pSIRbuw_mQb2z8ta5_XIYL_h3jMEaS8ydCItGHHJN1OSK17Yx2NFTQPP_91PSXgzr4chCLuCMyRFkfL63zhGChcCv5_X40f8y84
```
**Then you will see a window like, after signing in, add your desired sender Gmail account.**


<img width="331" height="323" alt="authenticatorappseting" src="https://github.com/user-attachments/assets/b0743a1e-e254-48ae-8add-18df33752b38" />

**    **

**Enter the app name, and a new window will appear as shown below.**

<img width="367" height="365" alt="authenticatorpassword" src="https://github.com/user-attachments/assets/df45188a-764a-45a7-a3cc-cc4988015585" />



**    **
## Step 3
**Download Python 3 on your Linux machine. To install Python 3, use:**
```
sudo apt install python3
```
**    **
## Step 4
**Once Python 3 is installed, create a Python file using sudo nano grpmaill.py.**
```
sudo nano grpmaill.py
```
**You may choose any name you prefer for the script.**
**    **
## Step 5
**Copy The Python script and past it into your terminal. Then, replace the Gmail address and receiver address with your own choices.**

```
#!/usr/bin/env python3
import os
import smtplib
import datetime
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders
from email.utils import make_msgid

# Config
SMTP_HOST = "smtp.gmail.com"
SMTP_PORT = 587
SENDER = "your_gamil.com"  # jsut put it your gmail here
SENDER_PASSWORD = os.getenv("GMAIL_APP_PASSWORD")  # set this in your shell No need to change here keep this as it is

RECEIVERS = ["abc3@gmail.com", "xyz.com.bd", "rocy@gmail.com"]
CC = []  # add CC addresses here
ALL_RECIPIENTS = RECEIVERS + CC

# Subject / body
today_date = datetime.date.today()
msg = MIMEMultipart()
msg["From"] = SENDER
msg["To"] = ", ".join(RECEIVERS)
if CC:
    msg["Cc"] = ", ".join(CC)
msg["Subject"] = f"Sub {today_date}"
msg["Message-ID"] = make_msgid()

html_body = """\
<html>
<body>
<p>Dear concern,</p>
<p>Your message. Please find the attached file.</p>
<p><em>This is an automatically generated email – please do not reply to it.</em></p>
<p>Best Regards,<br><strong>abcd</strong></p>
<hr>
<p style="color: #999999; font-size: 11px;">
<em>Disclaimer: Your message. Message</em></p>
</body>
</html>
"""
msg.attach(MIMEText(html_body, "html"))

# Attachment: ensure file exists
filename = "/home/kali/Desktop/python_tools/test.pdf"  # <- change to your real file path
if not os.path.isfile(filename):
    raise SystemExit(f"Attachment not found: {filename}")

with open(filename, "rb") as f:
    part = MIMEBase("application", "octet-stream")
    part.set_payload(f.read())
encoders.encode_base64(part)
part.add_header("Content-Disposition", f'attachment; filename="{os.path.basename(filename)}"')
msg.attach(part)

# Send email
if not SENDER_PASSWORD:
    raise SystemExit("GMAIL_APP_PASSWORD env var not set. Run: export GMAIL_APP_PASSWORD='your gmail password'")

try:
    with smtplib.SMTP(SMTP_HOST, SMTP_PORT, timeout=20) as smtp:
        smtp.ehlo()
        smtp.starttls()
        smtp.ehlo()
        smtp.login(SENDER, SENDER_PASSWORD)
        smtp.sendmail(SENDER, ALL_RECIPIENTS, msg.as_string())
    print("Email sent successfully")
except smtplib.SMTPAuthenticationError as e:
    print("Authentication failed:", e)
except Exception as e:
    print("Failed to send email:", type(e).__name__, e)
```
**Press Ctrl + X, then Y, and finally Enter to save and exit the file.**
**    **
## Step 6

<img width="529" height="49" alt="image" src="https://github.com/user-attachments/assets/09579ab5-26f1-4fa5-9e81-52ff42c4076b" />

**  **

**Set your Gmail App Password using the following command:**
```
export GMAIL_APP_PASSWORD="xxxxvvvvwwwwmmmm"
```
**      **
**This is the 16-digit password generated in Step 2 (App Password)**


<img width="367" height="365" alt="authenticatorpassword" src="https://github.com/user-attachments/assets/89aafe3f-5953-40cf-8e0d-61616ada940c" />


** **

## Step 7
**Once you’ve set the App Password, run the Python script to send the emails**
```
python3 grpmaill.py
```
**If you see the email sent successfully, it means all tasks were performed correctly and the process is complete**
<img width="684" height="84" alt="image" src="https://github.com/user-attachments/assets/267477cc-37c6-4b65-b1cd-70648324d82d" />

**    **
**Now check all the recipients’ email inboxes to make sure the mail was received**
**  **

   ## Thak you

**  **



























