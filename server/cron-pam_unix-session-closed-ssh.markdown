##Cron: pam_unix (cron:session): session opened/closed for user root by (uid=0)


This is my week of playing around with mail servers and I have been keeping an eye on the logs on a regular basis. I noticed that the auth.log was riddled with millions of these pointless (from my POV anyhow) log entries:
CRON: pam_unix(cron:session): session opened for user root by (uid=0)
CRON: pam_unix(cron:session): session closed for user root
This is - as is readily apparent - happening because of cron which can run every minute, every 10 minutes, every hour, and so on as configured. When cron does this running it often runs as root and doing so creates a session for said user. This, due to the default settings of most Linices, is logged (which does seem prudent if it wasn't so annoying) in auth.log. A kind soul on the Debian bug tracker has provided a solution that does not log this session activity, but only when run by cron. To do this (on Debian/Ubuntu):

1. Go to the `/etc/pam.d` directory.
2. Open the file `common-session-noninteractive` in an editor.
3. Look for the following line:

        session required        pam_unix.so
Above this line, add the following:

        session     [success=1 default=ignore] pam_succeed_if.so service in cron quiet use_uid
Save the file and exit.
4. Restart crond using something like 
    
        service cron restart.

Hope this helps :)