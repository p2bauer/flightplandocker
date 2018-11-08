This is the source Dockerfiles to build images for https://github.com/flightplan-tool/flightplan

**Updated**: 11/8/2018 and should automatically build to docker hub whenever Flightplan is updated. 

**What:**  
Docker images are like live CDs that create a virtual machine of a specific application.  In this instance it includes all of the dependencies and libraries (as well as the compiled binaries) to execute the Flightplan Tool.  This makes it run on anything and deploy very, very easily.

**How:**  
[Install Docker](https://docs.docker.com/install/)
Then, from your command prompt, you can run:

    sudo '#if you need it' \
    docker run \
        --rm '#this removes the container after run' \
        -it '#tell docker that we need to interact with it' \
        -v ~/projects/fpdata/config/:/app/config '#-v indicates a volume mapping. host directory on the left' \
        -v ~/projects/fpdata/data/:/app/data '#be sure to update all the mappings to where your local data is' \
        -v ~/projects/fpdata/db/:/app/db \ '#don't touch the volumes on the right'
        rbosworth/flightplan:cli '#this will pull the image from docker hub' \
        search --docker '#command to pass to flightplan -- you can use any of the cli commands'

Chromium has a TON of dependencies so this isn't a lightweight image. But docker does heavy caching so it won't use any more space once the server and client dockers are ready (it also makes it really easy/painless for it to stay up to date.)

If you want to make this easy to launch in the future you can add this to your ~/.bash_aliases file:

    alias fp='sudo docker run --rm -it -v ~/projects/fpdata/config/:/app/config -v ~/projects/fpdata/data/:/app/data -v ~/projects/fpdata/db/:/app/db rbosworth/flightplan:cli '

Then you can run the command from anywhere using "fp"

Windows users should be able to create a similar bat file and add it to your path.

**Docker Tags:**  
rbosworth/flightplan:cli -- Command line interface only
rbosworth/flightplan:client
rbosworth/flightplan:server

**Docker Compose:**  
The second two should be used with the following docker-compose.yml file:

    version: '2'
    services:
	    flightplanclient:
		    image: rbosworth/flightplan:client
			container_name: flightplanclient
			ports:
				- '3000:3000'
		flightplanserver:
			image: rbosworth/flightplan:server
			container_name: flightplanserver
			ports:
			    - '5000:5000'
		    volumes:
			    - ~/projects/fpdata/config:/app/config
				- ~/projects/fpdata/db:/app/db
			    - ~/projects/fpdata/data:/app/data
