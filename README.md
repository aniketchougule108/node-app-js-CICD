# Node.js App CI/CD Using Jenkins Webhook
### 🌐 Node.js App — Automated CI/CD Pipeline Using Jenkins + GitHub Webhooks
This project demonstrates how to automate the deployment of a Node.js application using Jenkins CI/CD and GitHub Webhooks. Whenever new code is pushed to the repository, Jenkins automatically triggers the pipeline to build, test, and deploy the updated version to the AWS EC2 target server — without any manual intervention.

![](./img/CICD.jpg)

This setup follows DevOps principles such as:
✔ Automation  
✔ Continuous Integration (CI)  
✔ Continuous Deployment (CD)  
✔ Version-controlled delivery  
✔ Zero-downtime app restart using PM2

---

### 🎯 Key Features

✅ Fully automated deployment using CI/CD  
✅ GitHub Webhooks for auto-triggered builds  
✅ No manual login to the target server  
✅ Builds and tests source code automatically  
✅ Deploys securely using SSH authentication  
✅ PM2 ensures uptime & auto-restores application  
✅ Suitable for production-grade environments

---

### 🔧 Tools & Technologies Used
<table border="1" cellpadding="10" cellspacing="0">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th>Tool</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Jenkins</td>
      <td>CI/CD automation</td>
    </tr>
    <tr>
      <td>GitHub</td>
      <td>Source control + Webhook triggers</td>
    </tr>
    <tr>
      <td>Node.js + npm</td>
      <td>Application runtime + dependency manager</td>
    </tr>
    <tr>
      <td>PM2</td>
      <td>Keeps app running in background</td>
    </tr>
    <tr>
      <td>AWS EC2 (Ubuntu)</td>
      <td>Deployment environment</td>
    </tr>
    <tr>
      <td>SSH Agent Plugin</td>
      <td>Secure authentication to server</td>
    </tr>
  </tbody>
</table>


### 🧩 Prerequisites

* GitHub Repository
* Jenkins Server running on Ubuntu
* SSH Key Authentication enabled
* Node.js App with app.js or server.js
* Jenkins Plugins:
    * GitHub
    * Pipeline
    * Git
    * SSH Agent
* Target EC2 server(node) with SSH access
* Node.js application source code hosted on a Git repository (e.g., GitHub).

---
## 🚀 Steps to Deploy
### ✅ Step 1 — Setup AWS EC2 Instances

Lauch two EC2s in same VPC(default)
* **Jenkins Server** → Add port 8080 in Security Group
* **Target Server** → Add port 22 & 3000 in security Group
install nodejs, npm and pm2 (manually)
```
sudo apt update
sudo apt install nodejs npm -y
sudo npm install -g pm2
```
![](./img/Screenshot%20servers.png)
---

### ✅ Step 2 — Create GitHub Repo

* Create a repository on Github
      * Name: `Nodejs-app-deploy-CICD`
      * Branch: `main`

![](./img/Screenshot%20repo.png)

#### Add Webhooks
Payload URL : http://<Jenkin-Server-PUBLIC-IP>:8080/github-webhook/

![](./img/Screenshot%20webhook.png)


---
    
### ✅ Step 3 — Jenkins Credentials
**Manage Jenkins → Credentials → System → Global:**

* Create New Credentails
    * Scope: `Global`
    * id: `node-app-key`
    * description: `node-app-key`
    * username: `ubuntu`
    * private key: `Your-Private-Key`

![](./img/Screenshot%20cred\).png)

---

### ✅ Step 4 — Create Jenkins Pipeline Job

* **Name** : node-app-deploy-cicd
* **item-type** : Pipeline

![](./img/Screenshot%20job.png)

* **Enable Trigger** : GitHub hook trigger for GITScm polling
* **Defination** : Pipeline script from SCM
* **SCM** : Git
* **Repository** : `https://github.com/aniketchougule108/Nodejs-app-deploy-CICD`
* **Branch** : main
* **Script Path** : `Your-jenkinsfile-name`

![](./img/Screenshot%20config.png)

---

### ✅ Step 5 — Write Jenkinsfile

![](./img/Screenshot%20jenkinsfile.png)

**NOTE:** Replace `SERVER_IP`, `SSH_CREDENTIAL`, `REPO_URL`, and `REMOTE_PATH` with your actual values.

---

### ✅ Step 6 — Push code and Jenkins file to Repository

push Nodejs application on github repository
```
git init
git add .
git commit -m ""
git push -u origin main
```
Now we pushed code to GitHub, a webhook instantly notifies the Jenkins server. Jenkins then automatically pulls the latest code, installs dependencies, runs tests, builds the application, and deploys it to the target server

![](./img/Screenshot%20build.png)

---

### ✅ Step 7 — Access Application

Open browser and enter http://< Node-Server-Public-Ip >:3000

![](./img/Screenshot%20deploy.png)

---

### 🛠️ Troubleshooting

<table border="1" cellpadding="10" cellspacing="0">
  <thead>
    <tr style="background-color: #d9edf7;">
      <th>Issue</th>
      <th>Fix</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Jenkins cannot SSH into EC2</td>
      <td>Check PEM permissions: <code>chmod 600 key.pem</code></td>
    </tr>
    <tr>
      <td>Webhook not triggering build</td>
      <td>Enable GitHub hook trigger &amp; Poll SCM</td>
    </tr>
    <tr>
      <td>App not running</td>
      <td>Check logs: <code>pm2 logs node-app</code></td>
    </tr>
    <tr>
      <td>Port not accessible</td>
      <td>Update AWS SG: open port <strong>3000</strong> inbound</td>
    </tr>
  </tbody>
</table>

---

### 🏁 Conclusion

This CI/CD setup delivers a fully automated, production-ready Node.js deployment:

✅ Faster releases  
✅ No manual deployment errors  
✅ Continuous & reliable delivery  
✅ Real DevOps workflow

It is a powerful example of how Jenkins, GitHub, PM2, and EC2 combine to streamline modern software delivery pipelines.

