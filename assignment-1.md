## 1. Conceptual Background:
For this assignment, I set up a virtual machine on CSC cPouta. The goal was to deploy a Virtual Private Server (VPS) and learn how IaaS works.

*   **What is IaaS and why to choose IaaS??**: 
* IaaS means Infrastructure as a Service. Wich in this case, CSC takes care of the physical hardware, virtualization and the main network infrastructure.
* Using IaaS makes me responsible for the things such as: the operating system, network settings and the applications running on the server.

In my case, I used Ubuntu as the operating system and Apache2 as the web server. I also configured the security group to allow the ports that I needed.
   
IaaS gives me more control over the server because I can install and configure the software myself. It can be used for websites, databases and other services. At the same time, I am also responsible for keeping the system updated and secure.

As an OAMK student, I have free access to CSC cPouta. This makes it a good environment for learning and testing cloud services without having to worry about the costs that can come with commercial cloud platforms.

---

## 2. Step-by-Step Implementation

### Step 1: Virtual Machine Provisioning
1. First I logged into my **CSC** account and opened the **cPouta** dashboard.
2. I created a new virtual machine with the following settings:
    *   **Instance Name:** `CloudServices-1`
    *   **Flavor:** `standard.medium` I picked this one because it provides a balanced amount of RAM and CPU
    *   **Operating System Image:** `Ubuntu 24`
    *   **Key Pair:** `CloudServices`
3. I also downloaded the private key file `CloudServices.pem`. This file is needed later so I'm able to connect to the VM using SSH through my console.

### Step 2: Firewall & Security Group Rules
By default, incoming traffic needs to be allowed through the cloud firewall before services on the virtual machine can be accessed. In the cPouta interface, I configured the security group with the following rules:
*   **Ingress TCP Port 22 (SSH):** Allows me to connect to the server remotely.
*   **Ingress TCP Port 80 (HTTP):** Allows normal web traffic to reach the Apache2 web server.
*   **Ingress TCP Port 443 (HTTPS):** Allows HTTPS traffic using SSL/TLS.
*   **Egress (All Ports):** I allowed outgoing traffic so the VM could connect to the internet, for example to download updates.

The internal IP address of the VM is 192.168.1.251. I assigned a public `Floating IP` address `86.50.230.220`. I used this IP to connect to the VM from my computer and later to open the webpage in my browser.

### Step 3: Server Connection & Installation
I use Fedora KDE Plasma on my laptop, so I used the built-in Konsole terminal to connect to the virtual machine through SSH instead of using Putty in Windows. Then I used the .pem key that I downloaded earlier.

```bash
ssh -i /home/anas/Documentos/OAMK/Cloud\ Services/Week\ 1/CloudServices.pem ubuntu@86.50.230.220
-i flag is to identity .pem file
```
### After connecting to the Ubuntu shell, I updated the packages and installed the Apache2 web server:

```bash
# Update the local package list
sudo apt update

# Install the Apache2 package
sudo apt install apache2

# Start and enable the service to make sure it runs continuously
sudo systemctl start apache2
sudo systemctl enable apache2
```

The start command starts **Apache2**, while enable makes sure the service starts automatically when the server boots.
I used systemctl status apache2 to check if the **Apache2** service was running correctly. The service was running, so I could continue with the web server configuration.

### Step 4: Customization & Verification
To see if Apache2 was installed correctly, I opened http://86.50.230.220 in my browser. I was able to see the default **Apache2** Ubuntu page, so when I saw that I knew that the web server was working.

After that, I wanted to change the default page a bit, so I opened the index.html file using:

```bash
sudo nano /var/www/html/index.html
```

I added a simple line of text to the file and saved the changes. Then I refreshed the page in the browser and could see the text that I added.

### Step 5: Screenshots & Proof of Work
(This screenshot shows the SSH connection to the Ubuntu virtual machine)
<img width="1916" height="948" alt="image" src="https://github.com/user-attachments/assets/7b0f626c-2593-4cd3-9398-644616b2a7ca" />
<img width="1919" height="953" alt="image" src="https://github.com/user-attachments/assets/05839a81-855c-426a-b9d6-3a4c1f3b4e2e" />



(This screenshot shows the customized index.html webpage being accessed through http://86.50.230.220)
<img width="1919" height="986" alt="image" src="https://github.com/user-attachments/assets/4b0d9dd8-526e-43ec-be70-feb78d1229ec" />


