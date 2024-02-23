## This note is for docker and kubernates
##

# limit localhost access for container
$ sudo docker run -p 127.0.0.1:1234:80 -d -v /var/www/:/usr/share/nginx/html nginx:1.9

# Docker restart policy
Docker provides restart policies to control whether your containers start automatically when they exit, or when Docker restarts. Restart policies start linked containers in the correct order. Docker recommends that you use restart policies, and avoid using process managers to start containers.
Restart policies are different from the --live-restore flag of the dockerd command. Using --live-restore lets you to keep your containers running during a Docker upgrade, though networking and user input are interrupted.
## Use a restart policy
To configure the restart policy for a container, use the --restart flag when using the docker run command. The value of the --restart flag can be any of the following:
## Flag	Description
* no	Don't automatically restart the container. (Default)
* on-failure[:max-retries]	Restart the container if it exits due to an error, which manifests as a non-zero exit code. Optionally, limit the number of times the Docker daemon attempts to restart the container using the :max-retries option. The on-failure policy only prompts a restart if the container exits with a failure. It doesn't restart the container if the daemon restarts.
* always	Always restart the container if it stops. If it's manually stopped, it's restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in restart policy details)
* unless-stopped	Similar to always, except that when the container is stopped (manually or otherwise), it isn't restarted even after Docker daemon restarts.
The following command starts a Redis container and configures it to always restart, unless the container is explicitly stopped, or the daemon restarts.
`
$ sudo docker run -d --restart unless-stopped redis
`
The following command changes the restart policy for an already running container named redis.
`
$ sudo docker update --restart unless-stopped redis
`
The following command ensures all running containers restart.
`
$ sudo docker update --restart unless-stopped $(docker ps -q)
`

# name and rename a container
## How to Name a Docker Container
You can assign memorable names to your docker containers when you run them, using the --name flag as follows. The -d flag tells docker to run a container in detached mode, in the background and print the new container ID.
`
$ sudo docker run -d --name discourse_app local_discourse/app
`
From now on, every command that worked with a container_id can now be used with a name that you assigned, for example.
`
$ sudo docker restart discourse_app
$ sudo docker stop discourse_app
$ sudo docker start discourse_app
`
## How to Rename a Docker Container
To rename a docker container, use the rename sub-command as shown, in the following example, we renaming the container discourse_app to a new name disc_app.
`
$ sudo docker rename discourse_app disc_app
`

# get container ID
`
$ id=$(sudo docker container list -f name=<container name> -q)
`

# run shell in a container
`
$ sudo docker exec -it <container id> /bin/sh
`
