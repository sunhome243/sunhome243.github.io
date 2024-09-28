---
title: "Workout App MVP: Personal Training with Technology"
categories:
  - projects
tags:
  - Major Projects
  - Solo Project
  - Backend
  - Frontend
  - Full Stack
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
toc_label: "Workout App MVP"
header:
  teaser: "/assets/posting/fitsync.png"
---

# Workout App MVP: Personal Training with Technology

## 📌 Project Overview and Goals

As the sole developer of this innovative workout application, I've taken on the challenge of maintaining personalized training consistency across different gyms and during solo workouts. This project addresses several key challenges I've identified in the personal training industry:

- Over 50% of clients discontinue using their personal trainers within six months
- Only 8% remain loyal for more than five years
- Clients struggle to maintain effective workouts independently
- Lack of access to previous session records
- Inconsistency when changing gym locations or trainers
- Reliance on offline records makes information transfer challenging

My solution provides:

- Customized training for every session, with or without a personal trainer
- Routines curated by trainers or AI-driven recommendations based on historical data
- Seamless training experience highlighting the value of personalized fitness coaching
- Improved client-trainer communication
- Efficient digital record-keeping
- AI-powered recommendations for solo workouts

## 🛠️ Technology Stack

After careful consideration and research, I've chosen a robust and modern technology stack for the MVP:

- **Backend**:

  - ![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
  - ![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
  - ![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
  - ![Firebase](https://img.shields.io/badge/-Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)

- **Frontend**:

  - ![Flutter](https://img.shields.io/badge/-Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
  - ![Dart](https://img.shields.io/badge/-Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white)

- **DevOps & Deployment**:
  - ![Docker](https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
  - ![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
  - ![Azure](https://img.shields.io/badge/-Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)

I selected these technologies based on their performance, scalability, and suitability for the project's requirements. FastAPI, for instance, was chosen for its high performance and ease of development, while PostgreSQL was selected for its robust data handling capabilities.

## 🏗️ Architecture Overview

I've designed the workout app on a microservices architecture, to be deployed on Azure Kubernetes Service (AKS). Here's a detailed look at the system architecture I've developed:

<table>
  <tr>
    <td><img src="/assets/posting/final-adjusted-kubernetes-architecture-svg.png" alt="app-structure" width="400"/></td>
  </tr>
</table>

### Key Components:

1. **Client Layer**: A Flutter-based mobile app for iOS and Android, which I've designed and implemented.
2. **Ingress Controller**: I've configured this to manage external access to the services in the cluster.
3. **Microservices**:
   - User Service: I've developed this to handle user management and profiles.
   - Workout Service: This service, which I've created, manages workout sessions and exercises.
   - Firebase Auth Service: I've integrated this for providing authentication and authorization.
4. **Database Pods**:
   - User Database: I've designed this to store user-related data.
   - Workout Database: I've structured this to store workout and exercise data.
5. **Kubernetes Tools**:
   - Prometheus: I'm in the process of setting this up for monitoring and alerting.
   - Helm: I plan to use this as a package manager for Kubernetes.
   - Grafana: I'll be implementing this for metrics visualization.

## 🚀 Implementation Details

### Backend Development

I've personally implemented the following features:

- User signup, login, and profile management using FastAPI.
- Workout session management and exercise logging capabilities.
- A trainer-user registration system for personalized training experiences.
- Quest session feature for trainers to assign workouts to members.
- Utilized PostgreSQL for robust and scalable data storage, with separate databases for user and workout data.
- Integrated Firebase for secure authentication, replacing the need for a custom auth service.

### Frontend Development

Using Flutter, I've created a responsive and intuitive user interface. Key screens I've designed and implemented include:

1. **Welcome and Signup Screens**:
   <table>
     <tr>
       <td><img src="/assets/posting/welcome_screen.png" alt="Welcome Screen" width="200"/></td>
       <td><img src="/assets/posting/signup.png" alt="Signup Screen" width="200"/></td>
       <td><img src="/assets/posting/signup_member.png" alt="Member Signup" width="200"/></td>
       <td><img src="/assets/posting/signup_trainer.png" alt="Trainer Signup" width="200"/></td>
     </tr>
   </table>

2. **Member Experience**:
   <table>
     <tr>
       <td><img src="/assets/posting/member_home.png" alt="Member Home" width="200"/></td>
       <td><img src="/assets/posting/member_profile.png" alt="Member Profile" width="200"/></td>
       <td><img src="/assets/posting/member_manage_trainer.png" alt="Manage Trainer" width="200"/></td>
       <td><img src="/assets/posting/member_all_session.png" alt="All Sessions" width="200"/></td>
     </tr>
   </table>

3. **Workout Experience**:
   <table>
     <tr>
       <td><img src="/assets/posting/member_workout_start0.png" alt="Workout Start" width="200"/></td>
       <td><img src="/assets/posting/custom_workout_exe.png" alt="Custom Workout Execution" width="200"/></td>
       <td><img src="/assets/posting/custom_workout_summary.png" alt="Workout Summary" width="200"/></td>
       <td><img src="/assets/posting/session_summary_custom.png" alt="Session Summary" width="200"/></td>
     </tr>
   </table>

4. **Trainer Experience**:
   <table>
     <tr>
       <td><img src="/assets/posting/trainer_home.png" alt="Trainer Home" width="200"/></td>
       <td><img src="/assets/posting/trainer_profile.png" alt="Trainer Profile" width="200"/></td>
       <td><img src="/assets/posting/trainer_all_sessions.png" alt="Trainer All Sessions" width="200"/></td>
       <td><img src="/assets/posting/trainer_workout_plan.png" alt="Trainer Workout Plan" width="200"/></td>
     </tr>
   </table>

5. **Custom Workout Creation**:
   <table>
     <tr>
       <td><img src="/assets/posting/custom_workout_create.png" alt="Create Custom Workout" width="200"/></td>
       <td><img src="/assets/posting/custom_workout_add.png" alt="Add Custom Workout" width="200"/></td>
       <td><img src="/assets/posting/custom_workout_review_plan.png" alt="Review Custom Workout Plan" width="200"/></td>
       <td><img src="/assets/posting/edit_workout_plan.png" alt="Edit Workout Plan" width="200"/></td>
     </tr>
   </table>

6. **Profile Management**:
   <table>
     <tr>
       <td><img src="/assets/posting/update_fitness_profile.png" alt="Update Fitness Profile" width="200"/></td>
       <td><img src="/assets/posting/update_member_info.png" alt="Update Member Info" width="200"/></td>
       <td><img src="/assets/posting/member_profile_imperial.png" alt="Member Profile Imperial" width="200"/></td>
       <td><img src="/assets/posting/member_profile_metric.png" alt="Member Profile Metric" width="200"/></td>
     </tr>
   </table>

### Database Schema

I've designed and implemented the PostgreSQL database schema, with separate schemas for the User Service and Workout Service. Here's a simplified overview of the database structure I've created:

#### User Service Schema

```python
class Member(Base, AsyncAttrs):
    __tablename__ = "members"
    uid = Column(String, primary_key=True, index=True)
    email = Column(String, unique=True, index=True)
    # ... other fields ...

class Trainer(Base, AsyncAttrs):
    __tablename__ = "trainers"
    uid = Column(String, primary_key=True, index=True)
    email = Column(String, unique=True, index=True)
    # ... other fields ...

class TrainerMemberMap(Base):
    __tablename__ = "trainer_member_mapping"
    id = Column(Integer, primary_key=True, index=True, autoincrement=True)
    trainer_uid = Column(String, ForeignKey("trainers.uid"))
    member_uid = Column(String, ForeignKey("members.uid"))
    # ... other fields ...
```

#### Workout Service Schema

```python
class Workouts(Base):
    __tablename__ = 'workouts'
    workout_id = Column(Integer, primary_key=True, autoincrement=True)
    workout_name = Column(String, nullable=False, unique=True)
    # ... other fields ...

class Session(Base):
    __tablename__ = "session"
    session_id = Column(Integer, ForeignKey("session_id_mapping.session_id"), primary_key=True)
    workout_key = Column(Integer, ForeignKey("workout_key_name_map.workout_key_id"), primary_key=True)
    # ... other fields ...

class Quest(Base):
    __tablename__ = 'quests'
    quest_id = Column(Integer, primary_key=True, autoincrement=True)
    trainer_uid = Column(String, nullable=False)
    member_uid = Column(String, nullable=False)
    # ... other fields ...
```

<table>
  <tr>
    <td><img src="/assets/posting/detailed-database-schema-svg.png" alt="Detailed Database Schema" width="1000"/></td>
  </tr>
</table>

### DevOps and Deployment

As the next phase of this project, I'm planning to set up the DevOps pipeline and deployment strategy:

- I plan to containerize services using Docker for consistency across environments.
- I will configure Kubernetes for orchestration, which will enable easy scaling and management.
- The deployment target will be Azure Kubernetes Service (AKS) for robust cloud infrastructure.
- Future steps include implementing Prometheus for monitoring and Grafana for visualization.


## 💡 Challenges and Solutions

Throughout this project, I've encountered and overcome several challenges:

1. **Database Schema Design**: Ensuring efficient data retrieval while maintaining relational integrity was challenging. I solved this by carefully normalizing and denormalizing data, and implementing efficient indexing strategies.

2. **Real-time Synchronization**: Keeping workout data synchronized across devices in real-time presented challenges. I implemented a robust WebSocket solution to ensure instant updates.

3. **Authentication Integration**: Switching from a custom auth service to Firebase Auth required careful integration and testing to ensure secure and seamless user authentication across all services. I dedicated significant time to understanding Firebase Auth's intricacies and ensuring smooth integration with my existing services.

## 🔮 Future Plans

As the sole developer of this project, I have focused plans for its future:

1. **AI-Powered Personal Trainer**:

   - Develop AI algorithms for personalized workout recommendations.
   - Target market: Busy professionals who can't afford regular personal training sessions.

2. **Corporate Wellness Programs**:

   - Introduce team challenges and company-wide fitness tracking.
   - Target market: Large corporations looking to improve employee health and productivity.

3. **Integration with Telehealth Platforms**:

   - Collaborate with telehealth providers to offer virtual fitness consultations.
   - Target market: Remote workers and individuals in rural areas with limited access to fitness professionals.

4. **Wearable Tech Synergy**:
   - Integrate with popular fitness wearables for comprehensive health tracking.
   - Target market: Tech-savvy fitness enthusiasts and quantified self advocates.

These plans aim to expand the app's reach beyond traditional gym-goers, tapping into new markets and addressing diverse fitness needs.

## 🎉 Conclusion

The completion of this MVP marks a significant personal achievement in my development career. I've single-handedly created a solid foundation for an innovative fitness application that has the potential to transform how people approach personal training and workout tracking. My focus on personalization, data-driven insights, and seamless user experience sets this app apart in the crowded fitness app market.

As I move forward, I'm excited about the possibilities for growth and improvement. I'm committed to continually refining the app based on user feedback and emerging technologies. My next steps include extensive user testing, iterating on the AI recommendations, and exploring partnerships with fitness centers and equipment manufacturers.

Stay tuned for more updates as I  to evolve and expand this application. I believe this project has the potential to make a real difference in people's fitness journeys, and I'm thrilled to be at the forefront of this exciting intersection of technology and health.