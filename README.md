**Gitleaks** is a SAST tool for detecting and preventing hardcoded secrets like passwords, api keys, and tokens in git repos. Gitleaks is an easy-to-use, all-in-one solution for detecting secrets, past or present, in your code.

Lets install **GitHub Pull Request Builder** and config it.

Skip installing :)

Go to ***Manage Jenkins > Configure System*** and find
![image](https://user-images.githubusercontent.com/40214066/187629832-fc329013-3068-4cd6-badd-fe373f9884e2.png)

Add credentials, username *github username* and instead of password write *Personal access tokens* from 

***Github > Settings > Developer settings > Personal access tokens > Generate new token***

![image](https://user-images.githubusercontent.com/40214066/187630589-9057816f-93ab-48dc-a7fd-b3d00e1cee19.png)

Tick all(for testing) and click *Generate Token*. Copy and save this token, we will use it later again. Then scroll down and tick Auto-manage webhooks. This will help us skip one step and not waste time on it. Apply and Save configs.

Then switch to Jenkins ans add new Pipeline job.

![image](https://user-images.githubusercontent.com/40214066/187631256-753a0927-3d78-4763-a53f-23405defd785.png)

Config triggers

![image](https://user-images.githubusercontent.com/40214066/187631318-df829ee4-5846-4ed4-8014-82c65a52e33c.png)

In ***Admin list*** we must write ***Github*** account user

*Trigger phrase* must be  `.*(re)?run tests.*`

Then config pipeline


![image](https://user-images.githubusercontent.com/40214066/187631898-57786bbe-d38f-488d-bb71-e00a7655b072.png)

Select your Credentials

***Refspec***  `+refs/pull/${ghprbPullId}/*:refs/remotes/origin/pr/${ghprbPullId}/*`

***Apply and Save***.

Config **Slack** notifications:

Then open Slack on your laptop and click Add apps

![image](https://user-images.githubusercontent.com/40214066/187633401-dccd145a-7a6a-4c07-b7aa-eebc8c284e44.png)

Select Jenkins from list and click Configuration. Web page will open. Click Add to Slack and then choose default channel for alerts.

![image](https://user-images.githubusercontent.com/40214066/187633532-aae00a62-d7fc-4f19-8e6f-ca219e7a6167.png)

And you will see 

![image](https://user-images.githubusercontent.com/40214066/187633591-e0aec47a-3289-430d-8024-54b8a1afc784.png)

Copy Token ID and click **SAVE**.

Install Slack Notification Plugin on Jenkins and click on Manage Jenkins again in the left navigation, and then go to Configure System. Find the Global Slack Notifier Settings section and add the following values:
Team Subdomain: turali7v 
Integration Token Credential ID: 
Create a secret text credential using rT8CpgqiNbdtJtdeAwnNMKkJ as the value
The other fields are optional. You can click on the question mark icons next to them for more information. Press Save when you're done.

![image](https://user-images.githubusercontent.com/40214066/187633841-ce2c026e-5615-49ad-9bcd-6529af1f4369.png)

## Note

For this pipeline we need some ***plugins*** and docker installed on jenkins host with sudo permissions:

```
sudo apt update
sudo apt upgrade
sudo apt-get install curl apt-transport-https ca-certificates software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo usermod -aG docker $USER
sudo usermod -aG docker jenkins
docker run hello-world
```
    
    
**Plungin list**: 
- Git
- Docker plugin
- Docker Pipeline
- GitHub plugin
- GitHub Pull Request Builder
- Slack Notification Plugin
