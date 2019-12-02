==============================
== Insalling the DST Server ==
==============================
 
- The following commands should be pasted into the PuTTY terminal in the order they appear. Lines with a hasktag (#) at the start are comments and shouldn't be pasted into PuTTY.
 
sudo dpkg --add-architecture i386
 
# this updates your new droplet
sudo apt-get update && time sudo apt-get dist-upgrade
 
# this installs needed dependencies to run the DST server
sudo apt-get install libstdc++6:i386 libgcc1:i386 libcurl4-gnutls-dev:i386
 
# this addes a new user named 'dst'
adduser dst
 
# this adds the new dst user to the sudo group
usermod -aG sudo dst

# SSH Keys
rsync --archive --chown=dst:dst ~/.ssh /home/dst
 
# this changes active terminal account from 'root' to 'dst'
su dst
 
# this installs steamcmd
mkdir ~/steamcmd
cd ~/steamcmd
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
 
# this makes folders for master and cave server shards
mkdir -p ~/.klei/DoNotStarveTogether/MyDediServer/Master
mkdir -p ~/.klei/DoNotStarveTogether/MyDediServer/Caves
 
# this creates your server token file, if you don't yet have your cluster token you'll need to create one
echo 'enter your server token here' > ~/.klei/DoNotStarveTogether/MyDediServer/cluster_token.txt
 
# Create your cluster.ini file
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/cluster.ini <<< 'W0dBTUVQTEFZXQpnYW1lX21vZGUgPSBzdXJ2aXZhbAptYXhfcGxheWVycyA9IDYKcHZwID0gZmFsc2UKcGF1c2Vfd2hlbl9lbXB0eSA9IHRydWUKCgpbTkVUV09SS10KY2x1c3Rlcl9kZXNjcmlwdGlvbiA9IFRoaXMgc2VydmVyIGlzIHN1cGVyIGR1cGVyIQpjbHVzdGVyX25hbWUgPSBTdXBlciBTZXJ2ZXIKY2x1c3Rlcl9pbnRlbnRpb24gPSBjb29wZXJhdGl2ZQpjbHVzdGVyX3Bhc3N3b3JkID0gCgoKW01JU0NdCmNvbnNvbGVfZW5hYmxlZCA9IHRydWUKCgpbU0hBUkRdCnNoYXJkX2VuYWJsZWQgPSB0cnVlCmJpbmRfaXAgPSAxMjcuMC4wLjEKbWFzdGVyX2lwID0gMTI3LjAuMC4xCm1hc3Rlcl9wb3J0ID0gMTA4ODkKY2x1c3Rlcl9rZXkgPSBzdXBlcnNlY3JldGtleQo='
 
# Create your Master server.ini
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/Master/server.ini <<< 'W05FVFdPUktdCnNlcnZlcl9wb3J0ID0gMTEwMDAKCgpbU0hBUkRdCmlzX21hc3RlciA9IHRydWUKCgpbU1RFQU1dCm1hc3Rlcl9zZXJ2ZXJfcG9ydCA9IDI3MDE4CmF1dGhlbnRpY2F0aW9uX3BvcnQgPSA4NzY4Cg=='
 
# Create your Caves server.ini
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/Caves/server.ini <<< 'W05FVFdPUktdCnNlcnZlcl9wb3J0ID0gMTEwMDEKCgpbU0hBUkRdCmlzX21hc3RlciA9IGZhbHNlCm5hbWUgPSBDYXZlcwoKCltTVEVBTV0KbWFzdGVyX3NlcnZlcl9wb3J0ID0gMjcwMTkKYXV0aGVudGljYXRpb25fcG9ydCA9IDg3NjkK'
 
# Create your Caves worldgenoverride.lua
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/Caves/worldgenoverride.lua <<< 'cmV0dXJuIHsKICAgIG92ZXJyaWRlX2VuYWJsZWQgPSB0cnVlLAogICAgcHJlc2V0ID0gIkRTVF9DQVZFIiwKfQo='
 
# Create the script that will run the servers.
base64 -di > ~/run_dedicated_servers.sh <<< 'IyEvYmluL2Jhc2gKCnN0ZWFtY21kX2Rpcj0iJEhPTUUvc3RlYW1jbWQiCmluc3RhbGxfZGlyPSIkSE9NRS9kb250c3RhcnZldG9nZXRoZXJfZGVkaWNhdGVkX3NlcnZlciIKY2x1c3Rlcl9uYW1lPSJNeURlZGlTZXJ2ZXIiCmRvbnRzdGFydmVfZGlyPSIkSE9NRS8ua2xlaS9Eb05vdFN0YXJ2ZVRvZ2V0aGVyIgoKZnVuY3Rpb24gZmFpbCgpCnsKICAgICAgICBlY2hvIEVycm9yOiAiJEAiID4mMgogICAgICAgIGV4aXQgMQp9CgpmdW5jdGlvbiBjaGVja19mb3JfZmlsZSgpCnsKICAgIGlmIFsgISAtZSAiJDEiIF07IHRoZW4KICAgICAgICAgICAgZmFpbCAiTWlzc2luZyBmaWxlOiAkMSIKICAgIGZpCn0KCmNkICIkc3RlYW1jbWRfZGlyIiB8fCBmYWlsICJNaXNzaW5nICRzdGVhbWNtZF9kaXIgZGlyZWN0b3J5ISIKCmNoZWNrX2Zvcl9maWxlICJzdGVhbWNtZC5zaCIKY2hlY2tfZm9yX2ZpbGUgIiRkb250c3RhcnZlX2Rpci8kY2x1c3Rlcl9uYW1lL2NsdXN0ZXIuaW5pIgpjaGVja19mb3JfZmlsZSAiJGRvbnRzdGFydmVfZGlyLyRjbHVzdGVyX25hbWUvY2x1c3Rlcl90b2tlbi50eHQiCmNoZWNrX2Zvcl9maWxlICIkZG9udHN0YXJ2ZV9kaXIvJGNsdXN0ZXJfbmFtZS9NYXN0ZXIvc2VydmVyLmluaSIKY2hlY2tfZm9yX2ZpbGUgIiRkb250c3RhcnZlX2Rpci8kY2x1c3Rlcl9uYW1lL0NhdmVzL3NlcnZlci5pbmkiCgouL3N0ZWFtY21kLnNoICtmb3JjZV9pbnN0YWxsX2RpciAiJGluc3RhbGxfZGlyIiArbG9naW4gYW5vbnltb3VzICthcHBfdXBkYXRlIDM0MzA1MCB2YWxpZGF0ZSArcXVpdAoKY2hlY2tfZm9yX2ZpbGUgIiRpbnN0YWxsX2Rpci9iaW4iCgpjZCAiJGluc3RhbGxfZGlyL2JpbiIgfHwgZmFpbCAKCnJ1bl9zaGFyZWQ9KC4vZG9udHN0YXJ2ZV9kZWRpY2F0ZWRfc2VydmVyX251bGxyZW5kZXJlcikKcnVuX3NoYXJlZCs9KC1jb25zb2xlKQpydW5fc2hhcmVkKz0oLWNsdXN0ZXIgIiRjbHVzdGVyX25hbWUiKQpydW5fc2hhcmVkKz0oLW1vbml0b3JfcGFyZW50X3Byb2Nlc3MgJCQpCgoiJHtydW5fc2hhcmVkW0BdfSIgLXNoYXJkIENhdmVzICB8IHNlZCAncy9eL0NhdmVzOiAgLycgJgoiJHtydW5fc2hhcmVkW0BdfSIgLXNoYXJkIE1hc3RlciB8IHNlZCAncy9eL01hc3RlcjogLycKCgo='
 
# Give the script executable permissions
chmod u+x ~/run_dedicated_servers.sh
 
# Run the script to start the dedicated servers
~/run_dedicated_servers.sh