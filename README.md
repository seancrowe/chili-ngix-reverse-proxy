# CHILI Apache Reverse Proxy
This is an example project to be able to see how a reverse proxy is setup for CHILI Publisher

No support will be given for this example.

# Setup

## Requirements
Before going to the setup steps, you need to:
- Download the project
- Install Docker and Docker Compose

### Download Project
Download this project using `git clone https://github.com/seancrowe/chili-apache-reverse-proxy.git` or downloading the code as a zip form github.

### Install Docker and Docker Compose
Install Docker and Docker Compose using your favorite package manager or from the docker website:

- Docker: https://docs.docker.com
- Docker Compose: https://docs.docker.com/compose/

Once both are installed, and your docker service is running, you are ready to move on to the setup steps.

## Setup Steps
All steps below assume you are in the project directory.

### Step 1
Open the nginx/nginx.conf file in your favorite editor and update the line 32 for your CHILI Publish Online/GraFx environment
```
proxy_pass https://ft-nostress.chili-publish.online/;
```

Important note, if you have more than one environment in the same region, then there is a very high chance you can use the same environment URL for all your environments.

So imagine if you have three environment URLs:
- `https://environment-1.chili-publish.online/`
- `https://environment-2.chili-publish.online/`
- `https://environment-3.chili-publish.online/`

There is a very high chance you can just setup a reverse proxy for any one of those, and it will work for all of them.

Example this setup, should work for environment-1 and environment-2:
```
proxy_pass /editor/ https://environment-3.chili-publish.online/;
```

Obviously, this is not true for production vs sandbox. For production and sandbox or different regions, you will need to setup different reserve proxies.

### Step 2
Run Docker Compose `up` with the `--build` option, which on Linux will most likely be the command:

```bash
sudo docker-compose up --build
```

### Step 3
If step 2 was successful, your website will be running on the IP address of the machine that is running Docker. In most cases, you can use localhost `127.0.0.1`

In a browser make sure to type in the IP with protocol HTTPS on port 8080.

Example: `https://127.0.0.1`

For most browsers, you will get a page a warning about "Your connection is not private", but just click `Advance` and the `Proceed` link.

A page will show with the header "This is a reverse proxy".

### Step 4
For the environment you want to test, grab a document ID, an API key with proper permissions created in that environment, and the environment name.

For example, with the base config, I would use `ft-nostress` for the environment.

Past your document ID, API key, and environment name into the input fields and press `Add Iframe`.

⚠️ Be careful that your document ID and API key are from the same environment, and that environment can be loaded with the base URL found in the `ngix.conf`.
