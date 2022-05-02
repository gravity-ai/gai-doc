# Sharing your data in a cluster

Occasionally, you might be in a situation where you need to spin up several of the same containers in a cluster and share data amongst them.
<br/>
<br/>
<b>For Example:</b>
<br/>
It'd be a pain to have to upload a license for each container and it might be especially annoying if you need to spin up the container 
dynamically.  In order to get around this problem, you can "share" your license key with other containers in your 
cluster via a docker volume mounted to your containers (assuming you're running locally), or via a shared EFS volume on AWS.  


## Local Setup
To do this locally, you can do the following using the CLI:

####1. <b>Create your shared volume.  I named it "shared-lic-vol" but you can call it whatever you want.</b>
```
$> docker volume create shared-lic-vol

shared-lic-vol
```
####2. <b>Verify that your volume was created sucessfully</b>
```
$> docker volume ls

DRIVER    VOLUME NAME
local     shared-lic-vol
```
####3. <b>Inspect it, in order to find the Mountpoint.</b>
```
$> docker volume inspect shared-lic-vol
[
    {
        "CreatedAt": "2022-04-29T21:40:59Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/shared-lic-vol/_data",
        "Name": "shared-lic-vol",
        "Options": {},
        "Scope": "local"
    }
]
```

####4. Run your first container, providing the environment variable GAI_VOLUME_PATH which should be where you mount your docker volume.  Note the -v flag, which is something like <i>name-of-volume:where-you-mount-that-volume</i>.  Finally, somewhat obviously, line continuation characters might vary depending on if you use powershell (windows), terminal (mac, linux, etc.).  You can just...all put it on the same line too, if you're into that sort of thing.

<b>One long string</b>
```
docker run -d --env GAI_VOLUME_PATH=/var/lib/docker/volumes/shared-lic-vol/_data -v shared-lic-vol:/var/lib/docker/volumes/shared-lic-vol/_data -p 7000:80 gx-images:t-995cb56d6c10472fa23649271c9d9b92
```

<b>Windows (powershell)</b>

```powershell
PS C:\Users\Me\Downloads> docker run -d --env GAI_VOLUME_PATH=/var/lib/docker/volumes/shared-lic-vol/_data `
>> -v shared-lic-vol:/var/lib/docker/volumes/shared-lic-vol/_data `
>> -p 7000:80 gx-images:t-995cb56d6c10472fa23649271c9d9b92
2f0e9d26447fcd6bf00980c133107fbb6d7e4bdc32e8aa46ad435b4d0efe16e8
```

<b>Linux/Mac (terminal)</b>

```bash
$> docker run -d --env GAI_VOLUME_PATH=/var/lib/docker/volumes/shared-lic-vol/_data \
> -v shared-lic-vol:/var/lib/docker/volumes/shared-lic-vol/_data \
> -p 7000:80 gx-images:t-995cb56d6c10472fa23649271c9d9b92
2f0e9d26447fcd6bf00980c133107fbb6d7e4bdc32e8aa46ad435b4d0efe16e8
```

####5. You can then create as many other containers as you wish, sharing that same volume and spun up in the same way. Just make sure you use different ports.

<b>Windows (powershell)</b>

```powershell
PS C:\Users\Me\Downloads> docker run -d --env GAI_VOLUME_PATH=/var/lib/docker/volumes/shared-lic-vol/_data `
>> -v shared-lic-vol:/var/lib/docker/volumes/shared-lic-vol/_data `
>> -p 7001:80 gx-images:t-995cb56d6c10472fa23649271c9d9b92
38468a3b35f7e32fc63d616499ba463f70759c6fdff6614096147aeca74ab00a
```

<b>Linux/Mac (terminal)</b>

```bash
$> docker run -d --env GAI_VOLUME_PATH=/var/lib/docker/volumes/shared-lic-vol/_data \
> -v shared-lic-vol:/var/lib/docker/volumes/shared-lic-vol/_data \
> -p 7001:80 gx-images:t-995cb56d6c10472fa23649271c9d9b92
38468a3b35f7e32fc63d616499ba463f70759c6fdff6614096147aeca74ab00a
```

## Afterwards
After you establish a shared volume amongst your containers, you are effectively sharing data between the containers.
<br/>
E.g. if you were to upload a license key on one running container, you should also see that license set for a container 
running that exact image. 