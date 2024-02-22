## This note is for docker and kubernates
##
# limit localhost access for container
'''
docker run -p 127.0.0.1:1234:80 -d -v /var/www/:/usr/share/nginx/html nginx:1.9
'''

# Docker restart policy
## Docker provides restart policies to control whether your containers start automatically when they exit, or when Docker restarts. Restart policies start linked containers in the correct order. Docker recommends that you use restart policies, and avoid using process managers to start containers.
## Restart policies are different from the --live-restore flag of the dockerd command. Using --live-restore lets you to keep your containers running during a Docker upgrade, though networking and user input are interrupted.
## Use a restart policy
## To configure the restart policy for a container, use the --restart flag when using the docker run command. The value of the --restart flag can be any of the following:
## Flag	Description
* no	Don't automatically restart the container. (Default)
* on-failure[:max-retries]	Restart the container if it exits due to an error, which manifests as a non-zero exit code. Optionally, limit the number of times the Docker daemon attempts to restart the container using the :max-retries option. The on-failure policy only prompts a restart if the container exits with a failure. It doesn't restart the container if the daemon restarts.
* always	Always restart the container if it stops. If it's manually stopped, it's restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in restart policy details)
* unless-stopped	Similar to always, except that when the container is stopped (manually or otherwise), it isn't restarted even after Docker daemon restarts.
## The following command starts a Redis container and configures it to always restart, unless the container is explicitly stopped, or the daemon restarts.
'''
$ sudo docker run -d --restart unless-stopped redis
'''
## The following command changes the restart policy for an already running container named redis.
'''
$ sudo docker update --restart unless-stopped redis
'''
## The following command ensures all running containers restart.
'''
$ sudo docker update --restart unless-stopped $(docker ps -q)**
'''
