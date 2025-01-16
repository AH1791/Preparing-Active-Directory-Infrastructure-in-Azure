# Preparing-Active-Directory-Infrastructure-in-Azure
<p align="center">
<img src="https://i.imgur.com/aMMGyHQ.jpeg" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />

<h1>Preparing Active Directory Infrastructure in Azure</h1>


<h2>Description</h2>
In this project I create two VMs (Virtual Machines), one running Windows Server, to act as a Domain Controller. The other VM will act as a client, running Windows 10 that will join the domain. In later projects I will deploy AD, run a script that will create users in the domain, which I can log into from the client VM, then manage the accounts and update the group policies, all to simulate a real life IT environment!  <br/>
<br />


<h2>Environments and Utilities Used</h2>

- <b>Microsoft Azure</b>
- <b>Virtual Machines</b>
- <b>Remote Desktop Connection</b>


<h2>Operating Systems Used </h2>

- <b>Windows Server </b>
- <b>Windows 10</b>

<h2>Project Walk-through:</h2>

<p align="center">
Navigate to Microsoft Azure and create a resource group: <br/>
 
![image](https://github.com/user-attachments/assets/5f496924-1147-432a-ab34-92d16ac75556)

<br />
<br />
Next, create a virtual network like so: <br/>

![image](https://github.com/user-attachments/assets/2da13d36-da7e-4a57-87c9-a11d15d88484)

<br />
<br />
Once my resource group and network is created, I'll create and set up the virtual machine that will act as our Domain Controller. For the image, make sure you use Windows Server:  <br/>

![image](https://github.com/user-attachments/assets/ec96a4a1-8a97-44fd-8eab-c4b04d534ec4)

<br />
<br />

![image](https://github.com/user-attachments/assets/bedd33ab-4053-446f-9ece-fae0fafd471c)

<br />
<br />
In the Networking tab of this VM, I'll make sure it will create itself on the virtual network I just created. I'll leave all other settings default and create this VM: <br/>

![image](https://github.com/user-attachments/assets/2edf47c8-e877-44ad-bbd3-7f4ed531a7a5)

<br />
<br />
Now, I'll create another VM that will serve as the client. The image for this machine should be Windows 10, NOT Windows Server like I did for the previous machine:  <br/>

![image](https://github.com/user-attachments/assets/74da2208-d2cd-449d-b78b-154cc8af5476)

<br />
<br />

![image](https://github.com/user-attachments/assets/1cadf145-114b-48ce-bd5f-2d646378211c)

<br />
<br />
In the Networking tab of this VM, I'll make sure it will create itself on the same virtual network of the previous machine created. I'll leave all other settings default and create this VM:  <br/>

![image](https://github.com/user-attachments/assets/f7e3c6e6-d396-463c-bc87-acd6c6c6ee35)

<br />
<br />
I now need to set our DC (Domain Controller) private IP address to "static" as by default it is set to "dynamic". I want this to be static, because this DC will double as a DNS (Domain Name System) server, which I will tell our client to use as a DNS server later. If the IP allocation setting were set to dynamic, the IP address could change leaving the DNS configuration of our client invalid. So, I'll go to the network settings of the DC and switch the IP allocation to static:  <br/>

![image](https://github.com/user-attachments/assets/1fa46dce-12c9-43c9-a854-b7cce95e000c)

<br />
<br />

![image](https://github.com/user-attachments/assets/fecf52c7-2857-4dcf-980d-df46e392c210)

<br />
<br />
Next, I'll use Remote Desktop Connection to connect to the DC using its public IP and the log in credentials I created when setting up this machine:  <br/>

![image](https://github.com/user-attachments/assets/21112103-c799-465f-a7b7-861726a267bc)

<br />
<br />
Once I'm logged in, the following screen will appear with the Server Manager open. (If this isn't what you're seeing and instead it a regular windows desktop, you may have connected to the client VM instead or chose the wrong image when creating the DC):  <br/>

![image](https://github.com/user-attachments/assets/1f97ad72-6626-4991-97b4-9b11503e0b16)

<br />
<br />
Next, I'm going to disable the firewall (you probably wouldn't do this in real lfe, but for the sake of this lab where nothing is at stake, I'll go ahead and do it). So, to disable the firewall I'll right click on the "Start" button and select "Run". Then type "wf.msc":  <br/>

![image](https://github.com/user-attachments/assets/f3ea5dfc-bc36-4b94-8aba-26ebc5c5dff6)

<br />
<br />
Click on "Windows Defender Firewall Properties" then on the, "Domain Profile", "Private Profile, and the "Public Profile" tabs, turn the firewall state off:  <br/>

 ![image](https://github.com/user-attachments/assets/79d12bae-4c6e-46d8-ba69-f473c26c48c6)

<br />
<br />

![image](https://github.com/user-attachments/assets/8ed47906-e70b-4019-a219-0a0489d57eb5)

<br />
<br />

![image](https://github.com/user-attachments/assets/7f8babaa-be6c-4c7d-91be-62d118a4768e)

<br />
<br />
You should see that all the firewall settings are disable:  <br/>

![image](https://github.com/user-attachments/assets/c52ba37f-21da-404f-94c0-4c7e999bfde3)

<br />
<br />
Next, I need configure our clients DNS settings to the DC. To start, back in Azure, I'll grab the DCs private IP address:  <br/>

![image](https://github.com/user-attachments/assets/48d56b7e-9511-4945-8fe6-c01e90180422)

<br />
<br />
Then, I'll go to the network setting of the client machine. click on the NIC (Network Interface Card), go to settings, then DNS servers and switch from "Inherit from virtual network" to "Custom". Input the DCs private IP here and save:  <br/>
<img
<br />
<br />
After that's saved, I'll restart the client machine:  <br/>
<img 
<br />
<br />
Once the machine as restarted, I'll use Remote Desktop connection to connect to the client machine using its public IP and the log in credentials I created while setting up this machine:  <br/>
<img 
<br />
<br />
Now that I'm logged in, I will open Powershell and attempt to ping the DC using the ping command and its private IP address. In my case it'll look like this. (If there is an error and the connection timed out, double check in Azure to make sure both of the machines are on the same virtual network. If they aren't this is likely causing the error and you'll need to set up the machine again on the same network):  <br/>
<img 
<br />
<br />
While I'm here I can double check that the DNS server settings are pointing to the DC. I'll run "ipconfig /all" and look for the "DNS Servers" and it should point to our DC if everything is set up properly:  <br/>
<img 
<br />
<br />

<h2>Active Directory Infrastructure is Now Prepared! </h2>

<b>We've successfully created two VMs (Virtual Machines), one running Windows Server, to act as a Domain Controller. The other VM as a client, running Windows 10. Don't forget: In later projects I will deploy AD, run a script that will create users in the domain, which I can log into from the client VM, then manage the accounts and update the group policies, all to simulate a real life environment!  </b>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
