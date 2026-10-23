Version: 1.0

Frontend
- GitHub Pages

Backend
- Google Apps Script

Proxy
- Cloudflare Worker

Database
- Google Sheets

Status
✔ Production Ready

Diagnostics

Browser time
= time from browser sending request
  until browser receives/processes response

Worker time
= time measured inside Cloudflare Worker

Server time
= time measured inside Apps Script

Browser : 2623 ms
Worker  : 2432 ms
Server  : 614 ms

Timings overall as below so total is not the sum of 3, total is timing as seen by user from click of button till response (also seen by user)

Browser:  ├────────────────────────────── 2623 ms ──────────────────────────────┤
          │                                                                      │
Worker:   │   ├────────────────────── 2432 ms ────────────────────────┤         │
          │   │                                                        │         │
Server:   │   │       ├──── 614 ms ────┤                              │         │
          │   │       └────────────────┘                              │         │
          │   └────────────────────────────────────────────────────────┘         │
          └──────────────────────────────────────────────────────────────────────┘

          Browser overhead outside Worker 2623 - 2432 = 191 ms
          Worker time outside Apps Script 2432 - 614 = 1818 ms

          Timing	Value	Meaning
Browser	2623 ms	End-to-end request as experienced by browser
Worker	2432 ms	Worker-side request duration
Server	614 ms	Apps Script processing
Browser − Worker	191 ms	Browser/network overhead around Worker
Worker − Server	1818 ms	Worker ↔ Apps Script / Worker-side overhead
Total	—	Separate user-perceived operation time


Browser       : 8003 ms
Worker        : -
Server        : -
Verification  : 8963 ms
Total         : 16973 ms

It tells us:

Browser waited 8 seconds for the check-in request, received no response, then entered the verification/recovery process.

==========================
Cloud flare path
https://dash.cloudflare.com/c92ae9e12630e5925c539f6ef11e2044/workers/services/edit/kkscanner-proxy/production

================================================
****Scanner Path
https://aly1kor.github.io/LoboScanner/********
===============================================================
Prepare Google Sheet
1. Add columns as per columns needed in config.
2. copy all the appscripts to the new google sheet
3. Open your Apps Script project, Click Project Settings ⚙️ on the left. Under General settings, find “Show appsscript.json manifest file in editor”. Enable/check that option. ensure timing is "timeZone": "Europe/Berlin",
4. Ensure Config.gs is updated with correct configs and sheet names, Google Sheet ID etc
5. Deploy the appscripts (always newly )
   Execute as: Me / User deploying
Who has access: Anyone

6. copy the exec appscripts url in cloudflare - Settings - Variables and secrets - APPS_SCRIPT_URL

Sending Invitation Email.

1. in "Test Email" column , make YES for test emails to be sent eg core team
2. EmailInvitation.gs select and run testInvitationEmail function
3. Check all ok edit if needed
4. Send Email to all participant, run sendAllInvitationEmails Function, email sent status is update in google sheet
5. if recipients >100 then rerun the step5  next day.

Filling Token

1. In appscripts click  Code.gs and  run onOpen - in google sheet you will get generate token button
2. Click  generate Tokens now token should be filled for all rows in google sheet


Appscripts config(EmailQR.gs sending)

1. in "Test Email" column , make YES for test emails to be sent eg core team
2. in appscripts click EmailQR.gs and run TestEmail function
3. Emails will be sent to test recipients, modify email content as needed
4.  Send Email to all participant, run sendAllEmails Function, email sent status is update in google sheet




    
Appscripts config(Feedback sending, After event )
Step 1 — 
Edit the Feedback.gs with needed questions as per the current event eg auction, housie etc, Create the form( this is best way to change to avoid issues in dash board due to chnages)
Run createFeedbackForm() once. This should create the form and save its ID in Script Properties.
Edit the form if needed and publish, you will see the changes immediately in view form link.

Step 2 — Open and link it
Run openFeedbackForm() to retrieve the form URLs, then run linkFeedbackForm() to link it to your existing spreadsheet.

Step 3 — Confirm the response tab
Find the new response tab in the spreadsheet, rename the sheet eg "FeedbackResponses" and enter its exact name into Config.gs CONFIG.FEEDBACK.RESPONSE_SHEET.

Step 4 — Test the feedback email
Send a test email and verify the form link, Registration ID, anonymity wording, and feedback-email tracking.

Step 5 — Test the dashboard
Run setupFeedbackDashboard(), submit a test response, and verify the dashboard refresh. Then configure setupFeedbackTrigger() to automate updates.
based on changes done in questionaire in step1 some changes may have to be done in FeedbackDashboard.gs based on headers in response sheet, search the error and find the new names

Step 5 — Automnatic trigger Select setupFeedbackTrigger in the function dropdown.
Click Run and authorize it when prompted.

   

