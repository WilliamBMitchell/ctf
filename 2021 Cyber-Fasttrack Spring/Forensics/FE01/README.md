# FE01
## BRIEFING
Download the file and find a way to get the flag.

Contents: fe01.ost

## Solution

After initially attempting to simply open the .ost file in Outlook (and it not working) I found a tool called Kernel OST Viewer which made short work of opening the file.

<img src="ost_viewer.png" width="650" >

I perused the mailbox directories a little bit and came across an interesting looking string in a meeting withing the Calendar section:

<img src="calendar_pass.png" width="650" >

Moving on, I scrolled through the emails in the Inbox and found one containing an attachment called flag.zip:

<img src="inbox.png" width="650" >

I double-clicked on the attachment and it opened up. I tried to extract the .jpg file within and was prompted for a password.

![protected](protected.png)

I entered the string from the Calendar meeting and the .zip file unzipped successfully.

<img src="flag.jpg" width="450" >

The flag is **pst_i'm_in_here!**
