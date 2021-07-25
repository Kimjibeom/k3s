# k3s-install on Raspberry PI

High-Performance Data Processing & Analysis Lab(HPC Lab) KIM JiBeom(김지범)



How to install k3s on Rasberry PI



![1](https://user-images.githubusercontent.com/73589723/122949326-87283580-d3b6-11eb-8564-e4ca3f334570.PNG)

When you run the Raspian, the initialization surface appears as above. Click Next.

![2](https://user-images.githubusercontent.com/73589723/122949520-ade66c00-d3b6-11eb-88bf-8927bc457511.PNG)

Select a country and click Next.

![3](https://user-images.githubusercontent.com/73589723/122950151-31a05880-d3b7-11eb-817e-5e1c4582d8f8.PNG)

click Next.

![4](https://user-images.githubusercontent.com/73589723/122950280-4aa90980-d3b7-11eb-834d-2d4f5bc9a548.PNG)

When entering Linux, modify the password default of raspberry to your own password and click Next.

![5](https://user-images.githubusercontent.com/73589723/122950421-657b7e00-d3b7-11eb-9b50-00dde2d41303.PNG)

Select the Wi-Fi to connect to the network and click Next.

![6](https://user-images.githubusercontent.com/73589723/122950531-7cba6b80-d3b7-11eb-9f53-2f1177032826.PNG)

If there is a password on the Wi-Fi, enter the password and press Next.

![7](https://user-images.githubusercontent.com/73589723/122950618-8f34a500-d3b7-11eb-94ca-6e9ef503bc79.PNG)

click Skip.

![8](https://user-images.githubusercontent.com/73589723/122950709-a2e00b80-d3b7-11eb-9d16-0b60bcd9835c.PNG)

Click the raspberry shape in the upper left corner of the rasbian and click Preferences-> Raspberry Pi Configuration

![9](https://user-images.githubusercontent.com/73589723/122951354-2dc10600-d3b8-11eb-98a4-5e41c58604bc.PNG)

In the Interfaces topic, change SSH and VNC to Enabled and click OK.

![10](https://user-images.githubusercontent.com/73589723/122952735-2b12e080-d3b9-11eb-9ac1-2c9d028e2a57.PNG)

Put the IP address in the Host Name entry and enter the Port you have assigned to Port.

![11](https://user-images.githubusercontent.com/73589723/122952751-2ea66780-d3b9-11eb-87c9-220d556e3eb1.PNG)

Connect as id:pi  pw:raspberry (default) input

# sudo raspi-config

![12](https://user-images.githubusercontent.com/73589723/122952759-31a15800-d3b9-11eb-9fa1-58a66c5a5ffb.PNG)

Select 1.System Options on the screen

![13](https://user-images.githubusercontent.com/73589723/122952786-36fea280-d3b9-11eb-89ab-d82723191389.PNG)

S4. Set Up Raspberry Hostname on Hostname

Checking Hostname.

# sudo nano /etc/hosts

![14](https://user-images.githubusercontent.com/73589723/122952836-39f99300-d3b9-11eb-9005-7c2fb8efbf18.PNG)

Check the name and ip of the raspberry pi.

Modify if different from configuration.

Enabling Linux Container Features.

# sudo nano /boot/cmdline.txt

![15](https://user-images.githubusercontent.com/73589723/122952864-3c5bed00-d3b9-11eb-9050-7a57af60f570.PNG)

Reboot after adding the following.

# cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory

install Docker.

![16](https://user-images.githubusercontent.com/73589723/122952869-3ebe4700-d3b9-11eb-9196-6ec99bc54640.PNG)

install docker

# sudo apt install docker.io

Start docker service

# sudo systemctl start docker

Start docker service on boot

# sudo systemctl enable docker

Check Docker Status

# sudo systemctl status docker

install k3s.

![17](https://user-images.githubusercontent.com/73589723/122952884-41b93780-d3b9-11eb-8246-f209a90c21ac.PNG)

install Script

# curl –sfL https://get.k3s.io | sh –

Check k3s Status

# sudo systemctl status k3s

![18](https://user-images.githubusercontent.com/73589723/122952901-441b9180-d3b9-11eb-85d7-3ea652286d00.PNG)

Verify Master Node is Live

# sudo k3s kubectl get nodes

![19](https://user-images.githubusercontent.com/73589723/122952915-47af1880-d3b9-11eb-946f-6b71977e8e51.PNG)

From Master Node – Get Token

# sudo cat /var/lib/rancher/k3s/server/node-token

Installing k3s on a Worker Node

![20](https://user-images.githubusercontent.com/73589723/122952926-4978dc00-d3b9-11eb-8836-e2b350b8e587.PNG)

Installation and Join

# sudo curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=XXX sh -

k3s-agent startup verification

![21](https://user-images.githubusercontent.com/73589723/122952938-4c73cc80-d3b9-11eb-95ed-c2a51268ed5e.PNG)

Confirm Join with Master Node

# Thank You

- 👋 Hi, I’m @Kimjibeom
- 👀 I’m interested in BIG DATA 
- 🌱 I’m currently learning ML, Edge Computing
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me a889997@naver.com
