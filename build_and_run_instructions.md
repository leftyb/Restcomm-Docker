# Restcomm Docker image build instructions

Maintainer George Vagenas - gvagenas@telestax.com

Restcomm binds to the ip address of the host and following ports:
- http: 8080
- sip/udp: 5080
- sip/tcp: 5080
- rtp/udp: 65000 - 65535

### Prerequisites
This image was tested with Docker 1.7


### Build

To build the image:

First git clone this repository and then:

```docker build -t mobicents/restcomm:latest -f Dockerfile .```

__Make sure you don't skip the dot (.) at the end of the command__

### Run

Download the [__restcomm_workspace__](https://github.com/Mobicents/Restcomm-Docker/src/f2c2fca52465f4c01a5c6af496e52b5d26f292b8/restcomm_workspace.zip?at=restcomm-smpp) that contains the default database, default RVD workspace and the required folders and unzip it to a folder in your filesystem.

Next run Restcomm image using the following volume arguments:

```docker run --name=restcomm --restart=always -d -e VOICERSS_KEY="YOUR_VOICESS_KEY_HERE" -p 8080:8080 -p 5080:5080 -p 5080:5080/udp -p 65000-65535/udp -v $YOUR_FOLDER/restcomm_workspace/restcomm/log:/opt/TelScale-Restcomm-JBoss-AS7/standalone/log -v $YOUR_FOLDER/restcomm_workspace/restcomm/recordings:/opt/TelScale-Restcomm-JBoss-AS7/standalone/deployments/restcomm.war/recordings -v $YOUR_FOLDER/restcomm_workspace/restcomm/cache:/opt/TelScale-Restcomm-JBoss-AS7/standalone/deployments/restcomm.war/cache -v $YOUR_FOLDER/restcomm_workspace/restcomm/data:/opt/TelScale-Restcomm-JBoss-AS7/standalone/deployments/restcomm.war/WEB-INF/data/hsql -v $YOUR_FOLDER/restcomm_workspace/mms/log:/opt/TelScale-Restcomm-JBoss-AS7/mediaserver/log -v $YOUR_FOLDER/restcomm_workspace/rvd/workspace:/opt/TelScale-Restcomm-JBoss-AS7/standalone/deployments/restcomm-rvd.war/workspace mobicents/restcomm:latest```


### To get bash console (for debugging only)

You can start the container and get a bash console to manually setup Restcomm and test it using the following command:

```docker run --name=restcomm --entrypoint=/bin/bash -it -p 8080:8080 -p 5080:5080 -p 5080:5080/udp mobicents/restcomm:latest```

### To get container logs

```docker logs restcomm```

### To execute a command at the container

```docker exec rc [command]```

For example

```docker exec rc ps -ef | grep java```
