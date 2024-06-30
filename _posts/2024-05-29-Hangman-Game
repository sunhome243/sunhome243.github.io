---
title: "The Hangman Game"
categories:
  - projects
tags:
  - Minor Projects
  - Independant Project
  - Backend
  - Frontend
  - Full Stack
  - Python
  - Flask
  - Javascript
  - Html
  - CSS
  - Docker
  - Kubernetes

sidebar:
  nav: "sidebar-category"
---

# The Hangman Game

## Project Description

Welcome to my very first project, Hangman—the classic word guessing game where players attempt to uncover a hidden word letter by letter. This web application brings the excitement of Hangman to your browser, featuring a collection of over 700 words for endless entertainment.

### **Features**

- **Word Database**: Enjoy a diverse selection of over 700 words, ensuring each game is unique and challenging.
- **Score Tracking**: Keep track of your progress with a built-in scoring system that records wins and streaks.
- **Player Persistence**: Resume your game right where you left off by reusing your player name to jump back into the action.

### Technologies Used

- **Frontend**: Designed with JavaScript, CSS, and HTML to provide an intuitive and engaging user experience.
- **Frontend**: JavaScript, CSS, and HTML
- **Backend**: Python (Flask)
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **CI/CD Pipeline**: Jenkins
- **Persistent Storage**: Azure (PV/PVC)

## Challenges and Learnings

Creating this web application was not without its challenges. Below are some of the issues I encountered and how I overcame them:

1. **Crash Loop Back Off Error:** This issue arose because I attempted to create images for each code without having any codes connecting each of the components. Resolving this required me to better understand the interaction between different components and ensure they were properly connected before creating images. 
2. **Second Crash Loop Back Off Error**: But this time, the log was 

```
 exec user process caused: exec format error
```

This was because the image was built on Arm (my MacBook) and was trying to run on Amd (GCP). I fixed this issue by building a multi-platform image that works on both Amd and Arm. 

```jsx
docker build --platform linux/amd64,linux/arm64 .
```

1. **Page Can Not Be Reached Error:** This error was due to my confusion about matching the ports. I needed to ensure that the ports in my code matched those specified in my Docker and Kubernetes configurations.
2. **Disabled 'Start' Button:** This was a major issue which took me over 6 hours to fix. The problem was that my JavaScript was not communicating with the Flask server. The solution was to consolidate the frontend and backend into a single Docker image, ensuring that the fetch function could establish a valid connection between them.
3. **Accessing the Service Externally:** Initially, I set the service type as a service. I learned that I should have set it to be a node port instead of a load balancer to allow external access.
4. **Image Tagging and Pushing:** I learned the importance of properly tagging the image with the format "username/imagename" and pushing it to the Docker registry.
5. **About:blank Error:** The service port ‘6000’ didn’t work on Azure. Very simple bug but was very hard to find. The flask server was running, all html, css, and js code was delivered to the port using the following code.

```bash
kubectl logs pods_name
```

```bash
curl externalip:port.codefile
```

I also checked NSG inbound rule to see if the port is allowed to be accessed. The final trial was using very simple Flask file to see if my Flask code gone wrong, but it also didn’t work. That’s when I found out my service is out of order. The simple change of the service port made it run everything in order.

**Why This happened is IMPORTANT!** There are several ports that are unavailable to use, and unfortunately, I used one of them. 

```
// The general list of blocked ports. Will be blocked unless a specific
// protocol overrides it. (Ex: ftp can use ports 20 and 21)
const int kRestrictedPorts[] = {
    1,       // tcpmux
    7,       // echo
    9,       // discard
    11,      // systat
    13,      // daytime
    15,      // netstat
    17,      // qotd
    19,      // chargen
    20,      // ftp data
    21,      // ftp access
    22,      // ssh
    23,      // telnet
    25,      // smtp
    37,      // time
    42,      // name
    43,      // nicname
    53,      // domain
    77,      // priv-rjs
    79,      // finger
    87,      // ttylink
    95,      // supdup
    101,     // hostriame
    102,     // iso-tsap
    103,     // gppitnp
    104,     // acr-nema
    109,     // pop2
    110,     // pop3
    111,     // sunrpc
    113,     // auth
    115,     // sftp
    117,     // uucp-path
    119,     // nntp
    123,     // NTP
    135,     // loc-srv /epmap
    139,     // netbios
    143,     // imap2
    179,     // BGP
    389,     // ldap
    427,     // SLP (Also used by Apple Filing Protocol)
    465,     // smtp+ssl
    512,     // print / exec
    513,     // login
    514,     // shell
    515,     // printer
    526,     // tempo
    530,     // courier
    531,     // chat
    532,     // netnews
    540,     // uucp
    548,     // AFP (Apple Filing Protocol)
    556,     // remotefs
    563,     // nntp+ssl
    587,     // smtp (rfc6409)
    601,     // syslog-conn (rfc3195)
    636,     // ldap+ssl
    993,     // ldap+ssl
    995,     // pop3+ssl
    2049,    // nfs
    3659,    // apple-sasl / PasswordServer
    4045,    // lockd
    6000,    // X11
    6665,    // Alternate IRC [Apple addition]
    6666,    // Alternate IRC [Apple addition]
    6667,    // Standard IRC [Apple addition]
    6668,    // Alternate IRC [Apple addition]
    6669,    // Alternate IRC [Apple addition]
    6697,    // IRC + TLS
};
```

Browsers block certain ports, although they are not in the system port range, some of them in the ranges widely used for local web development. After seeing this, I tried port 6666 for a test, and **IT DIDN’T WORK!** I wish I knew the earlier since I used 5 hours to figure it out what’s happening (even Chat GPT didn’t have the answer). 

1. **PV and PVC Addition:** I added PV on the Azure persistent disk, following the instructions. Then I created a PVC and its pod to work properly. There were some problem with name matching. **pvc.yaml’s ‘storageClassName’ and ‘volumeName’ should match with pv.yaml.**
2. **Not-updating-error:** This was a minor issue, fixed with very simple change. The deployment didn’t pull the new image even though I changed some features and pushed it. I added the following code to yaml 

```yaml
containers:
      - name: hangman-container
        image: whalerider02/hangman-app-mp:final
        imagePullPolicy: Always
```

Through these challenges, I learned the intricacies of developing a web application, containerizing it, and orchestrating with Minikube. It was a challenging but rewarding process.

## Updates

**05.23.2024** 

- Updated the docker image to a multi-platform image that will work with on both ARM and AMD architecture. This allows users to use this image on GCP.

**05.27.2024**

- Added Jenkins pipeline file for CI/CD.

**05.29.2024**

- Added user name input section at the start of the game.
- Added user score and streak feature to the game.
- Moved the platform from GKE to Azure.
- Added consistent volume and consistent volume claim on Kubernetes to save user data.