drupal-spamavoid
================

A technic to avoid spammers hitting your Drupal site.

In the last months on my site (Idolium), I observed that in the logs there was a lot of "access denied" items, all made in a few seconds from the same IP. The "user field" was full of strange names, that do not correspond to any registered user. Like this:

user  22/04/2013 - 19:01	Login attempt failed for larickjgd.	Anonymous	
user	22/04/2013 - 18:42	Login attempt failed for NiniBronsorge.	Anonymous	
user	22/04/2013 - 16:26	Login attempt failed for Weipscreese.	Anonymous	
user	22/04/2013 - 11:58	Login attempt failed for crynctymn.	Anonymous	
user	22/04/2013 - 06:48	Login attempt failed for hememaillaHew.	Anonymous	
user	21/04/2013 - 20:47	Login attempt failed for hojjanama.	Anonymous	
user	21/04/2013 - 15:19	Login attempt failed for hojjanama.	Anonymous

Obviously, all this is an spam try. But, if your server is not powerful, or your site is a bit slow, any request to your site makes it slower yet. And, of course, your log is full of garbage.

How can I avoid this. Well, my site has Mollom installed for registering a user, so for bots is hard to register as a user. And usually this bots use strange names to avoid duplicate a usual username, so, when mi site emails me with a new registered user, I can suspend the account if I suspect from the strange name.

So, basically, I have a safe user list, and I can access to drupal database to read the watchdog tables.
In other side, I can deny access to my site for certain IPs, using .htaccess file. And in the watchdog table I have the spammers's IPs.

Why not edit the .htaccess file to include all the spammers's IPs, lets say, every 12 hours? Well, I did it to test my theory, and the result was that the spam attemps has fallen 98%. Usually I had all my log full of spammers's attacks, but now I only find a few attacks, those which are from new IPs (until the next execution of the script, that will include the news).

Also I feel my site faster than before, which is logical, less petitions, more free CPU and less database access for logging.

On the bad side: if a real user wants to login (thinking that was registered before) and is not registered, he will be detected as spammer on the next execution of the script, unless he register himself after the failed logging attempt (which is the usual).

And what happens with the list of IPs? Well, for now will renew it automatically with the evolution of the watchdog records (I have 1 week config to discard logs, so in a maximum of 1 week, any banned IP will be allowed again, and if the spammer attacks again, in the next script execution will be included again). Remember that the script will re-create the .htaccess file every time, including the selected IPs from the SQL selection.

Finally, for those interested on my experiment, here are the keys:
- You can identify spammers accounts in your site (or you can avoid robots to create accounts, e.g., with Mollom), to delete it.
- You can use cron in your server to call a PHP script every X hours.

With this and the CheckIPs.php script, you can try my system and see what happens (always do a backup of .htaccess before test).
