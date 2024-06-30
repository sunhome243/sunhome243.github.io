---
title: "Fitness App - About This Project"
categories:
  - projects
tags:
  - major_projects
  - backend  

  nav: "sidebar-category"
---

- Techniques Used
    - Backend: Fast API
    - Frontend: Flutter or Swift or React Native (UI design exported from Figma)
    - Database: PostgreSQL
    - Database Migration: Alembic
    - Containerize: Docker
    - Container Orchestration: Kubernetes
    - CI/CD: Github Action
    - Used DevOps method

- **Why I chose Fast API**
    
    Fast API is a relatively new Python-based backend library. I considered Gin (GO), Flask (Python), Django (Python), Fast API (Python), and NodeJS (JavaScript). 
    
    My primary considerations were:
    
    - **High performance**
        - Since users will use the app during their workouts, fast response time is crucial and the top priority.
    - **Easy to learn and develop**
        - This being my second backend project (and first major one), ease of learning and development was crucial.
    - **Easy to expand (capable of Micro-service architecture)**
        - Scalability is important to easily add new features without complexity. Hence, micro-service architecture was chosen, requiring a flexible framework.
    
    I was initially interested in GO and am familiar with Python, which led me to narrow down my choices to Gin, Flask, Django, and Fast API. However, Django seemed too heavy for our prototype app, and Flask lacked built-in features, type checking, and asynchronous processing. This narrowed my choices down further to Gin and Fast API.
    
    Gin is a high-performance library (slightly faster than Fast API), ideal for micro-service architecture, lightweight, and simple. It presented a great opportunity to delve deeper into GO and Gin. However, considering our project's need for machine learning features and our team's familiarity with Python, I ultimately chose **Fast API**. It's easy to learn, high-performing, has a rich ecosystem, provides automatic documentation (Swagger), and supports async/await.
    
- **Why I chose PostgreSQL**
    
    I required high-performance capabilities for both reading and writing data, making PostgreSQL the ideal choice. PostgreSQL utilizes Multi-Version Concurrency Control (MVCC) for enhanced performance with concurrent transactions without heavy locking. It supports foreign data wrappers, robust concurrency control, and handles complex queries well, making it suitable for handling the demands of machine learning applications while maintaining relational integrity.

![Drawing2](https://github.com/sunhome243/sunhome243.github.io/assets/107231365/b6983548-020e-4add-9ff8-78a28d216922)



### **Problem Identification:**

Why do clients invest in expensive personal trainers? The answer is simple: to learn proper exercise techniques and achieve better health. The personal training industry is rapidly growing, projected to increase by 2.8% annually from 2020 to 2025. The demand is also expected to rise by 15% from 2019 to 2029, with the global personal training industry valued at approximately $10 billion (Worldmetrics, 2024).

However, not all clients can sustain ongoing payments to trainers indefinitely. Research indicates that over half of clients discontinue using their personal trainers within six months, with only 8% remaining loyal for more than five years (Insure4Sport, 2018). Given that personal training services typically cost between $300 to $500 per month, it becomes evident that many clients face financial constraints when engaging in personal training. Despite these challenges, clients require a reliable way to maintain effective workouts independently, especially when they do not have access to a personal trainer.

Many clients encounter challenges when exercising alone, such as lacking previous session records and struggling to remember weights and reps used during training sessions. Because trainers often record sessions on paper or in personal memo apps, accessing these records consistently can be difficult. These experiences can lead clients to question the value of personal training sessions and, ultimately, may lead them to discontinue using them.

Additionally, clients often experience inconvenience when changing gym locations due to job transitions, moving residences, or seeking a better gym nearby. Each change in personal trainer disrupts the continuity of their progress records. Consequently, until a new trainer adjusts and accumulates new data, clients do not experience truly personalized training.

These challenges often stem from trainers relying on offline records such as paper notes or memory, which makes transferring information effectively challenging. Furthermore, during solo workouts without a trainer present, accessing detailed information from previous sessions—such as weights used, number of sets, and rest times—is also challenging.

### **Solution:**

We propose an innovative app designed to maintain personalized training consistency across different gyms and during solo workouts. Members will receive customized training every session, whether they are working with a personal trainer or exercising independently. They can follow routines curated by their trainers or receive AI-driven recommendations based on their historical session data, including weights, repetitions, and sets.

This solution aims to deliver a seamless training experience that highlights the value of personalized fitness coaching while improving client-trainer communication. By maintaining continuity and personalization, the app enhances the member experience and empowers trainers to better guide their clients’ fitness journeys. Through efficient digital record-keeping, trainers can focus more on providing support, fostering a supportive and effective training environment. For clients, this app ensures they can achieve their fitness goals with confidence and consistency, whether at the gym or exercising independently, supported by personalized routines and expert-level AI recommendations.

### **Key Features:**

1. **Truly Personalized Training:** Members receive consistently personalized training sessions, ensuring continuity even when changing trainers or gyms. Trainers can easily gather feedback and tailor the experience for each client.
   
2. **Digital Record Keeping:** Transitioning from offline-based to digital records simplifies data management and enhances access to past training details.
   
3. **AI-Powered Recommendations:** AI algorithms suggest personalized workout routines based on historical session data, optimizing workouts for individual fitness goals. High-quality workout data from personal training sessions improves the overall quality of recommendations, setting us apart from competitors.
   
4. **Trainer Support Tools:** Tools for trainers to efficiently manage and customize member workouts, fostering a more supportive training environment.
   
5. **Enhanced Member Experience:** Enables members to follow tailored routines independently or under PT guidance, ensuring a seamless and effective fitness journey.