
Postmortem Nginx Server Issue
This is a postmortem document about the death archive approximately a server that required to be fixed since of a blunder in its configuration.
 Issue Summary: 
The server down was issued on the day 27th of January 2022, it takes approximately about approximately almost around 6 hours to be fixed. 
The issue was avoiding the server from running so the associations were disabled and there was no Nginx benefit accessible, so all clients were influenced including the admins. 
Root Cause:
 There were 3 fundamental failures within the arrangement of the server: 
The Nginx arrangement records in /etc/nginx/ had no appropriate client permissions. In arrange to run a ngix server, the nginx client ought to be able to get to these files with studied and compose permissions. 
The Nginx setup records didn't have the correct ownership. 
In the setup record /etc/nginx/sites-enabled/default was a bad setup, more often than not, the server ought to permit wage associations in port 8080, but it was set to port 80. 
There was an Apache process running and utilizing the required port.

Timeline
 The issue was identified at 9 a.m on 27th in January 2022, it was recognized by confirming the current handle running on the server. I utilized the ps -auxf command, there was no Nginx handle running, moreover check with sudo benefit nginx status and the yield appeared that it was disabled.
Debugging, Activities, and prepare to settle the Issue: - 

 To begin with, I attempted to run the benefit with the command sudo service nginx start, the server was running but it has issues to work, the associations were not permitted on port 8080. So I ceased the benefit and changed the arrangement record: /etc/nginx/sites-enabled/default. - 
I realized that the nginx forms weren't running from the correct client, and checked for the envelope consents, at that point I changed it, too changed possession of the arrangement envelope /etc/nginx/ .
At that point, I killed the apache server that was running to avoid conflicts with a special flat to be sure that the previous process with the same name were able .

 Finally, restart the nginx service.
 Commands used: 
sudo chmod uo+rw /etc/nginx/nginx
conf sudo chown nginx:nginx -R /etc/nginx/
 sed -i "s/80/8080/" /etc/nginx/sites-enabled/default 
pkill -o apache2
 su nginx -c "service nginx restart"
 Debugging the service forms was sufficient to discover the problem. (what parts of the framework were explored, what were the presumption on the root cause of the issue).
Things that can be improved/fixed: - 
The original arrangement of the server ought to be correctively done set as it were for nginx server running, not any other web service to avoid clashes and issues. - 
When setting nginx guarantee the possession and consents to be accurately set, the user and gather for this can be nginx. 
List of assignments to address the issue: 

Patch nginx server permissions and ownership folder & files.
kill other web server services running.
