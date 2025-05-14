# 🚀 Newsletter Platform Exploration

---


## 1.[🏗️ Create a Site Using Popular Newsletter Platforms <img src="https://images.saasworthy.com/tr:w-200,h-140,c-at_max,q-95,e-sharpen-1/beehiiv_41353_logo_1670833873_dlw9d.png" alt="beehiiv" width="50"/>](beehiiv.md)

### 🎯 Why i choose the platform
- Good UI compared to MailerLite and Substack
- [LearnXOps](https://www.learnxops.com) was hosted in beehiiv so i gave it a try.
- 2500 free mail
- Market Leader for newsletter publishing platform

### 🔍 How i explored it
- Got my Live Website in minutes
- Subscribed from Another mail
- Publish Article and send it via mail to subscribers
- Export Content and Subscribers
- Subscribe Gateway to read Blogs
- Email associated with my domain
- Send confirmation mail to user while subscribing and welcome mail.

[Create site using beehiiv <img src="https://images.saasworthy.com/tr:w-200,h-140,c-at_max,q-95,e-sharpen-1/beehiiv_41353_logo_1670833873_dlw9d.png" alt="beehiiv" width="50"/>](beehiiv.md)

### ⚡ Challenges faced
- SignUp: i was not getting OTP while SignUp, but got it after few minutes
- After mapping my domain and entering the URL in the browser, I initially landed on my Beehiiv site. However, the URL in the address bar showed the old one. After a few minutes, it updated to the correct URL.
- I don't have much design sense, took time in choosing colors.
- I wanted to Integrate Premium feature, but i don't have stripe account.

### 🧠 Learnings and observations
- Beehiiv uses S3 bucket to store Content and subscribers list, when in expored the data got the AWS S3 link.
- Configure to send mail with my domain
- Exporting content gives CSV file which cannot be directly imported to ghost, and exporting subscribers also gave a CSV file which can be directly imported to ghost.

---


## 2. [🛠️ Build Your Own Self-Hosted Newsletter Site <img src="https://user-images.githubusercontent.com/65487235/157849205-aa24152c-4610-4d7d-b752-3a8c4f9319e6.png" alt="ghost" width="50"/>](ghost.md)

### 🎯 Why i choose the platform
- Open Source/Free/Community
- Easy to start with minimal configuration with almost no time
- Documentation is eassy to understand
### 🔍 How I explored it
- Explored as a subscriber
- Publisher
- Edit the content
- Send Mail with my custom domain with Mailgun, tried to do forget password with mailgun integration (it said magic link failed).
### ⚡ Setup steps and challenges faced
- Migration from beehiiv to own Ghost CMS
- Setting mail was challanging.
- Path to store as volume: it is very important to know which path one should store as volume
- External DB: Database outside the ghost.

### 🧠 learnings and observations
- How Configurations are stored :: Path to be stored `/var/lib/ghost/content`
    - data/ghost.db
    - images/
    - themes
    - logs
    - settings
    - theme/   # custom-themes
- Mailgun
- Cannot import CSV file (which i exported from beehiiv) direclty to ghost used the tool called [migrate](https://github.com/TryGhost/migrate) 


---

### 3. 🐋 [Running Ghost CMS in Docker ](docker.md)


## 4. ☸️ [Running Ghost CMS in EKS ](Kubernetes/README.md)

### 🎯 Why i choose the platform
- Kubernetes easy to manage
    - application
    - networking
    - Storage/volume 

### 🔍 How I explored it
- Configure application using environment variables (ConfigMap)
- Created DB outside Ghost (Ghost has SQLite3 default for local development)
- Ingress for External access
- SSL with Let's Encrypt
- EBS for External Volume
- Sealed secret controller for secret encryption

### ⚡ Setup steps and challenges faced
- Import export
- ebs-csi-driver
- Certificate

### 🧠 learnings and observations
- How to manage Volume for containers, so that when new pod is created it has all the previous settings (using Persistance Volume)
- [MYSQL operator for Kubernetes](./opt/MYSQL-cluster.md)
- Add-on EBS-CSI-Driver
- Let's Encrypt free SSL for HTTPS.


**See also** [My site on Beehiiv](https://vishals-newsletter-25f774.beehiiv.com/)
