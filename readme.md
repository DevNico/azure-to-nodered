# Todo

## Prerequisites

- Azure Account

- Github Account

- [Git](https://git-scm.com/downloads)

- [Node.js](https://nodejs.org/en/download/)

- [Visual Studio Code](https://code.visualstudio.com/download) with Azure IoT Tools extension



## 1. Create IoT Hub

- Login to the https://portal.azure.com/

- Search for IoT Hub

- Click on "Create"

- Choose your subscription you have => probably already one in place

- Its always a good idea to create resource groups, so you can delete resources in the cloud by just deleting the whole resource group and keep an eye on the costs

- Enter your name for the IoT Hub

- As region we should choose the one closest to our operational area of the device to reduce lag => in our case most probably Germany West Central.

- Under Network settings we keep the IoT Hub public

- Under pricing and scaling tier we want the F1 - Free Tier

- Click on "Review + Create"



## 2. Create Device in IoT Hub

Once we have the IoT Hub ready, we create a device. You can use the Azure Portal as described here, or you can also use the Azure IoT Tools Extension in Visual Studio Code

- Go to your IoT Hub resource

- Under device management you should see "Devices"

- Click on "Add Device"

- Enter a unique Device ID

- To keep it simple, we choose the symmetric key authentication for the device. Real devices would use a more sophisticated authentication method, e.g. with x.509 certificates



## 3. Generate SAS Token from IoT Hub

- In Visual Studio Code you should have installed the Azure IoT Tools extension

- In the Explorer in Visual Studio Code you should see a tab with "AZURE IOT HUB"
  
  ![](https://github.com/keklireply/azure-to-nodered/blob/main/snips/image1.png)

- By right clicking on the device, you can click on "Generate SAS Token for your Device". You will be asked how many hours the Key will be valid, just add enough time for how long you want to use it.

- Keep the generated Key somewhere, we need it later to add it into our Node-Red flow to authenticate ourself



## 3. Setup Github Repository

If not done yet, you need to register to Github first to create a new repository. A great tool to have with lots of free features like Github Actions, unlimited private repositories. 

You can fork the following repository to have a base for Node-Red:

[GitHub - keklireply/azure-to-nodered: Little exercise to connect node red to an Azure IoT hub](https://github.com/keklireply/azure-to-nodered)

- Once you opened the link, you should see "Fork" on the top right. This will make a copy of the repository and add it to your own account

- For cloning and later also for Node-Red projects we need an SSH Keypair. Please follow the steps described in the Github Documentation: [Generating a new SSH key and adding it to the ssh-agent - GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

- Now we open the terminal in your preferred folder on your computer and clone the repository
  
  ```bash
  # When cloning with SSH
  git clone git@github.com:yourGithubUsername/iot-exercise.git
  
  
  # When cloning with HTTPS
  git clone https://github.com/yourGithubUsername/iot-exercise.git       
  ```

## 4. Setup Node-Red

- Change with your terminal into the repository that you cloned

- Please make sure you installed NodeJS then you should be able to run the following command
  
  ```bash
  npm install
  ```

- This will download and install Node-Red in this folder. You can also install Node-Red globally on your computer by running
  
  ```bash
  npm install -g --unsafe-perm node-red
  ```

- After successful installation you can start Node-Red by running
  
  ```bash
  npm run start
  ```

- Now you should see in the terminal that Node-Red started and you should be able to access node red now by your Internet Browser by typing in:
  
  ```url
  127.0.0.1:1880
  
  
  or
  
  
  localhost:1880
  ```

- Once you are in Node-Red you can click on the top right burger symbol and add a new Project

- We will clone the repository locally that we have forked before

- It will ask for the SSH Key we created beforehand (and the passphrase if you added one)

- Now you should see a flow with some nodes in it

## 5. Setup Connection to Azure IoT Hub

- On the MQTT Out node you need to change the topic to your own Device ID which you created before

- Under the server settings you need to change the name of the IoT Hub to the one you created

- You need to enable TLS and again add your Device ID as the ClientID below

- Under the tab "Security" you need to add the Username and Password as follows
  
  ```
  Username: YOURIOTHUBNAME.azure-devices.net/DEVICEID
  Password: SAS_TOKEN (previously generated)
  ```

- After deploying the changes by clicking on the top right red button, the Nodes should have now a green dot indicating that they are now connected to the MQTT broker => Its sending in data now!

## 6. Update the Node-Red flow

Now comes the good part: We can do updates on the flow and commit and push our changes to the remote repository in Github and share with everyone who as access to it, so you can collaborate better as a team.



- Change something in the flow, like adding a random node or renaming something

- On the right you can see a typical "Git" symbol that looks a bit like a tree

- ![](https://github.com/keklireply/azure-to-nodered/blob/main/snips/image2.png)

- There you can see the changes you have done and add the changes you want to commit to your repository

- When commiting, you need to also add a message which should include the changes you did in a concise way

- Under "Commit-History" you can now see if there are changes from the remote repository or commits you have still in place
  
  ![](https://github.com/keklireply/azure-to-nodered/blob/main/snips/image3.png)

- By clicking on it you can either pull or push changes. Remember to always pull before you push something



## Home Task or something for your project

Imagine running Node-Red now permanently on your device, e.g. Raspberry Pi. The Node-Red node "exec" can actually run terminal commands on your device and you could inject a event, e.g. every 20 seconds to pull the latest changes from the remote repository, basically a simple automatic OTA update :) 

Commands you will need:

```bash
cd ~/.node-red/projects/YOURPROJECTNAME &&  git pull origin main && npm run reload
```

Nodes:

- inject

- exec




