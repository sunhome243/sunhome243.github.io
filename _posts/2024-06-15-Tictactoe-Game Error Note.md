---
title: "The Tic-Tac-Toe Game Error Note"
categories:
  - errors
tags:
  - Minor Projects
  - Independant Project
  - Backend
  - Frontend
  - Full Stack
  - Python
  - Flask
  - Javascript
  - HTML
  - CSS
  - Docker
  - Kubernetes

sidebar:
  nav: "sidebar-category"

---

Errors that I have encountered during a minor project to create web-based Tic-Tac-Toe Game


## Challenges and Learnings

### **1. Fixing `docker: command not found` in Jenkins Pipeline**

I encountered the **`docker: command not found`** error in my Jenkins pipeline, it likely means Jenkins is running in an environment that does not have Docker in its **`PATH`**. To resolve this issue, I needed to add Docker's path to Jenkins' environment variables.

**Steps to Add Docker to Jenkins PATH:**

**1. Find Docker's Path**

First, ensure you know where Docker is installed on your system. Run the following command in your terminal:

```bash
which docker
```

This command will likely return **`/usr/local/bin/docker`**.

**2. Edit Jenkins Environment Variables**

To add Docker's path to Jenkins' environment variables, follow these steps:

1. **Open Jenkins Dashboard**:
    - Open your web browser and navigate to your Jenkins dashboard. This is typically at **`http://localhost:8080`** if you are running Jenkins locally.
2. **Navigate to Configure System**:
    - From the Jenkins dashboard, click on **Manage Jenkins** on the left-hand side menu.
    - Then click on **Configure System**.
3. **Set Environment Variables**:
    - Scroll down to the **Global properties** section.
    - Check the box labeled **Environment variables**.
    - Click the **Add** button to add a new environment variable.
4. **Add Docker Path to Environment Variable**:
    - In the **Name** field, enter **`PATH`**.
    - In the **Value** field, enter **`/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin`** (ensure to include any existing paths if necessary).
    - This ensures Jenkins can locate the Docker executable.
5. **Save the Configuration**:
    - Scroll to the bottom of the page and click **Save** to apply the changes.

**Additional Steps if Needed:**

After updating the environment variables, it's a good practice to restart Jenkins to ensure the changes take effect.

> **Restart Jenkins from the Jenkins Web Interface**:
> 
- Navigate to **Manage Jenkins** > **Safe Restart** or **Reload Configuration from Disk**.

> **Restart Jenkins from the Command Line**:
> 
- If you installed Jenkins using Homebrew, you can restart it with the following commands:

```bash
brew services restart jenkins-lts
```

### 2. Permission denied Error

Jenkins keep showed permission denied on buildx directory. I fixed it with this code

`sudo chmod -R g+rx /Users/rocket/.docker/buildx
brew services restart jenkins`

### 3. Welcome to Nginx!

Nginx keep showed me this, not the html I needed. I needed to fix the config file of the nginx. 

1. Access to the nginx cotainer

`kubectl exec -it <pod name> -- sh`

1. Change directory and access to the config file

`cd /etc/nginx/conf.d`

`vi default.conf`

1. Change the config file (add the boldeed text)

```
server {

listen       80;

listen  [::]:80;

server_name  localhost;

#access_log  /var/log/nginx/host.access.log  main;

**location / {
    root   /usr/share/nginx/html/templates;
    index  index.html index.htm;
}

location /static/ {
    alias /usr/share/nginx/html/static/;
}

location /templates/ {
    alias /usr/share/nginx/html/templates/;
}**

#error_page  404              /404.html;

# redirect server error pages to the static page /50x.html
#
error_page   500 502 503 504  /50x.html;
location = /50x.html {
    root   /usr/share/nginx/html;
}

# proxy the PHP scripts to Apache listening on 127.0.0.1:80
#
#location ~ \\.php$ {
#    proxy_pass   <http://127.0.0.1>;
#}

# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
#location ~ \\.php$ {
#    root           html;
#    fastcgi_pass   127.0.0.1:9000;
#    fastcgi_index  index.php;
#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
#    include        fastcgi_params;
#}

# deny access to .htaccess files, if Apache's document root
# concurs with nginx's one
#
#location ~ /\\.ht {
#    deny  all;
#}
}
```

The point was to put index.html (the basic html code) to the root. Because there was nothing on root file directory, the Nginx failed to render the html.

### 4. Seeing Nothing on the Webpage.

No error message, No log, Nothing. It worked perfectly on local. The problem was here.

```python
PROJECT_ROOT = os.path.abspath(os.path.dirname(**file**))
static_path = os.path.join(PROJECT_ROOT, 'frontend/static')
templates_path = os.path.join(PROJECT_ROOT, 'frontend/templates')

app = Flask(**name**, static_folder=static_path, template_folder=templates_path)
```

It was copying the code from the current directory, but the backend and frontend was on different pod, making absolute no sense to copy the file from the directory that doesn’t even exists. So I used 

app.py

```python
static_path = os.getenv('FRONTEND_STATIC_PATH', 'frontend/static')
templates_path = os.getenv('FRONTEND_TEMPLATES_PATH', 'frontend/templates')
app = Flask(__name__, static_folder=static_path, template_folder=templates_path
```

backend-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
name: tictactoe-backend
spec:
replicas: 1
selector:
matchLabels:
app: tictactoe-backend
template:
metadata:
labels:
app: tictactoe-backend
spec:
containers:
- name: backend
image: whalerider02/tictactoe-backend:latest
imagePullPolicy: Always
env:
- name: FRONTEND_STATIC_PATH
value: /usr/share/nginx/html/static
- name: FRONTEND_TEMPLATES_PATH
value: /usr/share/nginx/html/templates
ports:
- containerPort: 5000
```

This uses environment variable to give the specific path, and if there is no ev, it uses default path.