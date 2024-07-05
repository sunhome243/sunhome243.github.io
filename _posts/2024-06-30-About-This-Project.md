---
title: "Workout App - About This Project"
categories:
  - projects
tags:
  - Major Projects
  - Team Project
  - Backend
  - Python
  - Fast API
  - Postgre SQL
  - Docker
  - Kubernetes

sidebar:
  nav: "sidebar-category"

toc: true
toc_sticky: true
toc_label: "The Fitness App Project"
---
# 🏋️‍♀️ Workout recording app - Major Project

## 📖 Brief Project Description

Welcome to my first major project

 our second project—a fully functional web application for the timeless game of Tic-Tac-Toe. This interactive platform brings the classic board game to life with a user-friendly interface and robust functionality.

## 📋 Table of Contents

### 🛠 Technology Choices
- [Why I chose Fast API](#-why-i-chose-fast-api)
- [Why I chose PostgreSQL](#-why-i-chose-postgresql)

### 🔄 Project Development
- [Initial Algorithm Flow List](#-initial-algorithm-flow-list)

### 📊 Project Overview
- [Problem Identification](#-problem-identification)
- [Solution](#-solution)
- [Key Features](#-key-features)

## 🛠️ Technologies Choices

### Backend
![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)

### Frontend
![Flutter](https://img.shields.io/badge/-Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Swift](https://img.shields.io/badge/-Swift-FA7343?style=for-the-badge&logo=swift&logoColor=white)
![React Native](https://img.shields.io/badge/-React_Native-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Figma](https://img.shields.io/badge/-Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white)

### Database
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Alembic](https://img.shields.io/badge/-Alembic-000000?style=for-the-badge&logo=alembic&logoColor=white)

### DevOps & Deployment
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/-GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![DevOps](https://img.shields.io/badge/-DevOps-0A0A0A?style=for-the-badge&logo=devops&logoColor=white)

## 🧩 Initial Algorithm Flow List
![Drawing2](https://github.com/sunhome243/sunhome243.github.io/assets/107231365/b6983548-020e-4add-9ff8-78a28d216922)


### 🚀 Why I chose Fast API

Fast API is a relatively new Python-based backend library. I considered Gin (GO), Flask (Python), Django (Python), Fast API (Python), and NodeJS (JavaScript). 

My primary considerations were:

- **High performance** 🏎️
    - Since users will use the app during their workouts, **fast response time** is crucial and the top priority.
- **Easy to learn and develop** 📚
    - This being my second backend project (and first major one), **ease of learning and development** was crucial.
- **Easy to expand (capable of Micro-service architecture)** 🔀
    - **Scalability** is important to easily add new features without complexity. Hence, **micro-service architecture** was chosen, requiring a flexible framework.

I was initially interested in GO and am familiar with Python, which led me to narrow down my choices to Gin, Flask, Django, and Fast API. However, Django seemed too heavy for our prototype app, and Flask lacked built-in features, type checking, and asynchronous processing. This narrowed my choices down further to Gin and Fast API.

Gin is a high-performance library (slightly faster than Fast API), ideal for micro-service architecture, lightweight, and simple. It presented a great opportunity to delve deeper into GO and Gin. However, considering our project's need for **machine learning features** and our team's familiarity with Python, I ultimately chose **Fast API**. It's easy to learn, high-performing, has a rich ecosystem, provides **automatic documentation** (Swagger), and supports **async/await**.

### 🐘 Why I chose PostgreSQL

I required high-performance capabilities for both reading and writing data, making PostgreSQL the ideal choice. PostgreSQL utilizes **Multi-Version Concurrency Control (MVCC)** for enhanced performance with concurrent transactions without heavy locking. It supports **foreign data wrappers**, robust **concurrency control**, and handles **complex queries** well, making it suitable for handling the demands of machine learning applications while maintaining relational integrity.

## 📊 Project Overview

### 🔍 Problem Identification:

The personal training industry is rapidly growing, projected to increase by **2.8% annually** from 2020 to 2025. The demand is also expected to rise by **15% from 2019 to 2029**, with the global personal training industry valued at approximately **$10 billion** (Worldmetrics, 2024).

However, challenges exist:

- Over **half of clients discontinue** using their personal trainers within six months
- Only **8% remain loyal** for more than five years (Insure4Sport, 2018)
- Personal training services typically cost between **$300 to $500 per month**
- Clients face difficulties in **maintaining effective workouts independently**
- **Lack of access** to previous session records
- **Inconsistency** when changing gym locations or trainers
- Reliance on **offline records** (paper notes or memory) makes information transfer challenging

### 💡 Solution:

We propose an innovative app designed to maintain **personalized training consistency** across different gyms and during solo workouts. Key aspects include:

- Customized training for every session, with or without a personal trainer
- Routines curated by trainers or AI-driven recommendations based on historical data
- Seamless training experience highlighting the value of personalized fitness coaching
- Improved client-trainer communication
- Efficient digital record-keeping
- AI-powered recommendations for solo workouts

### ✨ Key Features:

1. **Truly Personalized Training:** 
   - Consistently personalized training sessions
   - Continuity when changing trainers or gyms
   - Easy feedback gathering for trainers

2. **Digital Record Keeping:** 
   - Transition from offline to digital records
   - Simplified data management
   - Enhanced access to past training details

3. **AI-Powered Recommendations:** 
   - Personalized workout routines based on historical data
   - Optimization for individual fitness goals
   - High-quality recommendations from personal training session data

4. **Trainer Support Tools:** 
   - Efficient management of member workouts
   - Customization options for trainers
   - Fostering a supportive training environment

5. **Enhanced Member Experience:** 
   - Tailored routines for independent or PT-guided workouts
   - Seamless and effective fitness journey
   - Consistent personalization across different workout scenarios