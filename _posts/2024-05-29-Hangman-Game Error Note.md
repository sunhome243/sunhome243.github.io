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

---

A minor project to create web-based Hangman Game

## Project Description

Welcome to my very first project, Hangman—the classic word guessing game where players attempt to uncover a hidden word letter by letter. This web application brings the excitement of Hangman to your browser, featuring a collection of over 700 words for endless entertainment.

## Technologies Used

- **Frontend**: JavaScript, CSS, and HTML
- **Backend**: Python (Flask)
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **CI/CD Pipeline**: Jenkins
- **Persistent Storage**: Azure (PV/PVC)


## **Features**

- **Word Database**: Enjoy a diverse selection of over 700 words, ensuring each game is unique and challenging.
- **Score Tracking**: Keep track of your progress with a built-in scoring system that records wins and streaks.
- **Player Persistence**: Resume your game right where you left off by reusing your player name to jump back into the action.

![Photo 1](/assets/posting/Hangman.png)
![Photo 2](/assets/posting/Hangman2.png)

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