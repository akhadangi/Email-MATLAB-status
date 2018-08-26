# Email-MATLAB-status
This gorgeous code checks the status of MATLAB and sends an email notification 
to two email addresses, whether still running or just finished. You can specify
the intervals this code checks the status. The default is set to 60 minutes i.e.
this code checks MATLAB status every 60 minutes and sends its current status to
email addresses specified.

NOTE: DO NOT USE SMTP OF GMAIL (smtp.gmail.com) i.e. DO NOT USE YOUR GMAIL ADDRESS
TO SEND NOTIFICATION TO THOSE TWO ADDRESSES, because Gmail will not allow to
authenticate via the Python. Use other domains like your university email address
which usually have less restrictions

_-_-_-_-_-_-_-_-_- USAGE  _-_-_-_-_-_-_-_-_-
run E-MATLAB.py
