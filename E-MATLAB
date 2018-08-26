# -*- coding: utf-8 -*-
"""
Created on Sun Aug 26 18:28:42 2018

@author: Afshin Khadangi
"""

import schedule
import time
import datetime
import psutil
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
'''
This gorgeous code checks the status of MATLAB and sends an email notification 
to two email addresses, whether still running or just finished. You can specify
the intervals this code checks the status. The default is set to 60 minutes i.e.
this code checks MATLAB status every 60 minutes and sends its current status to
email addresses specified.

NOTE: DO NOT USE SMTP OF GMAIL (smtp.gmail.com) i.e. DO NOT USE YOUR GMAIL ADDRESS
TO SEND NOTIFICATION TO THOSE TWO ADDRESSES, because Gmail will not allow to
authenticate via the Python. Use other domains like your university email address
which usually have less restrictions

'''
MATLAB = [];    #define a list to store running MATLAB pids
for proc in psutil.process_iter():     #search through all the current processes
     try:
         pinfo = proc.as_dict(attrs=['pid', 'name', 'username', ])     #store process info into dic
         if pinfo.get('name') == 'MATLAB.exe':          #find details of processes as MATLAB.exe
             MATLAB.append(pinfo)                       #list the dictionaries
             p = psutil.Process(pinfo.get('pid'))       #get pid of MATLAB and make a process object          
     except psutil.NoSuchProcess:
         pass
def job():                                              #define the process which meets our schedule
    msg = MIMEMultipart()
    msg2 = MIMEMultipart()
    fromaddr = "From-mail address: username@domain.com"
    toaddr1 = "To-mail address 1:  username1@domain.com"
    toaddr2 = "To-mail address 2:  username2@domain.com"
    
    msg['From'] = fromaddr
    msg['To'] = toaddr1
    msg['Subject'] = "YOUR SUBJECT"
    msg2['From'] = fromaddr
    msg2['To'] = toaddr2
    msg2['Subject'] = "YOUR SUBJECT"
    cpuusage = p.cpu_percent(interval=1)                #get the cpu usage of MATLAB
    
    if cpuusage > 50:                                   #define the threshold of MATLAB usage (usually above 20)
        body1 = "Your MATLAB code is still running, IF NOT, I will let you know!\nThis message is generated on Python. The date and time of most recent call is: " + str(datetime.datetime.now())
        body2 = "Your MATLAB code is still running, IF NOT, I will let you know!\nThis message is generated on Python. The date and time of most recent call is: " + str(datetime.datetime.now())
        msg.attach(MIMEText(body1, 'plain'))
        msg2.attach(MIMEText(body2, 'plain'))
        server = smtplib.SMTP('smtp.domain.com', 589)  #589 is an arbitrary port here, check with your domain
        server.ehlo()
        server.starttls()                               #start LTS with SMTP server
        server.login("username@domain.com", "password")       #some servers only accept username, not combined like username@domain.com
        text = msg.as_string()
        server.sendmail(fromaddr, toaddr1, text)            #send the email to address 1
        server.quit()
        
        server = smtplib.SMTP('smtp.domain.com', 589)
        server.ehlo()
        server.starttls()
        server.login("username@domain.com", "password")
        text2 = msg2.as_string()
        server.sendmail(fromaddr, toaddr2, text2)           #send the email to address 1
        server.quit()
        print("Your MATLAB code is still running, the date and time of most recent call is:", datetime.datetime.now())  #optional
    else:
        body1 = "THE PROCESSING IS DONE!\nThis message is generated on Python. The date and time of most recent call is: " + str(datetime.datetime.now())
        #here the message is set to inform the recepients that running has been finished
        body2 = "THE PROCESSING IS DONE!\nThis message is generated on Python. The date and time of most recent call is: " + str(datetime.datetime.now())
        msg.attach(MIMEText(body1, 'plain'))
        msg2.attach(MIMEText(body2, 'plain'))
        server = smtplib.SMTP('smtp.domain.com', 589)
        server.ehlo()
        server.starttls()
        server.login("username@domain.com", "password")
        text = msg.as_string()
        server.sendmail(fromaddr, toaddr1, text)
        server.quit()
        
        server = smtplib.SMTP('smtp.domain.com', 589)
        server.ehlo()
        server.starttls()
        server.login("username@domain.com", "password")
        text2 = msg2.as_string()
        server.sendmail(fromaddr, toaddr2, text2)
        server.quit()
    
    return

schedule.every(60).minutes.do(job)              #specify the interval of notificaion

while True:
    schedule.run_pending()
    time.sleep(60) # wait 60 minutes until the next check
