# Docker issue: WARNING you have Transparent Huge Pages (THP) support enabled

https://github.com/docker-library/redis/issues/55

Leaving some more detail for this in case other Docker for Mac users still have this problem.

This solved the above problem with the following system info:
Docker version 17.12.0-ce, build c97c6d6
Mac OS X El Captain (10.11.6)
Redis version=4.0.8

This is a very naive solution this problem (mostly just cutting and pasting from SO and above, so use at your own risk).

Step 1: Start a screen session the linux VM that docker for mac is running on

$ screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty
(h/t https://stackoverflow.com/questions/39739560/how-to-access-the-vm-created-by-dockers-hyperkit)

Step 2: Run the commands above

\# echo never > /sys/kernel/mm/transparent_hugepage/enabled
\# echo never > /sys/kernel/mm/transparent_hugepage/defrag
Step 3: Exit screen session, and run whatever docker images / commands.