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
  - HTML
  - CSS
  - Docker
  - Kubernetes

sidebar:
  nav: "sidebar-category"

toc: true
toc_sticky: true
toc_label: "Hangman Project"

header:
  teaser: "/assets/posting/Hangman2.png"
---

🎮 Hangman Web Application - Minor Project

![Hangman Game Banner](https://via.placeholder.com/1500x300.png?text=Hangman+Web+Game)

## 📖 Project Description

Welcome to my very first project—a web-based version of the classic word-guessing game, Hangman! This application brings the excitement of Hangman to your browser, featuring a robust collection of over 700 words for endless entertainment.

## 🛠️ Technologies Used

![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![CSS3](https://img.shields.io/badge/-CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![HTML5](https://img.shields.io/badge/-HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/-Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Jenkins](https://img.shields.io/badge/-Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Azure](https://img.shields.io/badge/-Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)

## ✨ Features

- 📚 **Word Database**: Enjoy a diverse selection of over 700 words, ensuring each game is unique and challenging.
- 📊 **Score Tracking**: Keep track of your progress with a built-in scoring system that records wins and streaks.
- 💾 **Player Persistence**: Resume your game right where you left off by reusing your player name to jump back into the action.

## 📸 Screenshots

<table>
  <tr>
    <td><img src="/assets/posting/Hangman.png" alt="Hangman Screenshot 1" width="400"/></td>
    <td><img src="/assets/posting/Hangman2.png" alt="Hangman Screenshot 2" width="400"/></td>
  </tr>
</table>

## 💻 Code Repositories

This project is available in two versions:

1. **Kubernetes Version**: 
   - Repository: [https://github.com/sunhome243/Hangman-Webapp](https://github.com/sunhome243/Hangman-Webapp)
   - This version includes all necessary Kubernetes settings for deployment on a cluster.

2. **Local Version**:
   - Repository: [https://github.com/sunhome243/Hangman-web-app-local](https://github.com/sunhome243/Hangman-web-app-local)
   - This version can be run directly on your local computer without Kubernetes.

Choose the version that best fits your needs and development environment!

## 🚀 Getting Started

### For Kubernetes Version:

**You need to use Azure Kubernetes service (AKS) and set PV to run Kubernetes Version**

1. Clone the repository:
   ```
   git clone https://github.com/sunhome243/Hangman-Webapp.git
   ```
2. Navigate to the project directory:
   ```
   cd Hangman-Webapp
   ```
3. Set up AKS and PV, and deploy.

### For Local Version:

1. Clone the repository:
   ```
   git clone https://github.com/sunhome243/Hangman-web-app-local.git
   ```
2. Navigate to the project directory:
   ```
   cd Hangman-web-app-local
   ```
3. Run:
    ```
    python main.py
    ```

## 🔄 Recent Updates

### May 29, 2024
- ✨ Added user name input section at the start of the game.
- 🏆 Implemented user score and streak feature.
- 🚀 Migrated the platform from GKE to Azure.
- 💾 Integrated consistent volume and consistent volume claim on Kubernetes to save user data.

### May 27, 2024
- 🔧 Added Jenkins pipeline file for CI/CD.

### May 23, 2024
- 🐳 Updated the Docker image to a multi-platform image compatible with both ARM and AMD architectures, enabling usage on GCP.
