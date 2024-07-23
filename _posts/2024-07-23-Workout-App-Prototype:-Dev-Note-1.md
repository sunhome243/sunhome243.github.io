---
title: "Workout App - Dev Note 1"
categories:
  - projects
tags:
  - Major Projects
  - Team Project
  - Backend
  - Frontend
  - Full Stack

sidebar:
  nav: "sidebar-category"

toc: true
toc_sticky: true
toc_label: "The Fitness App Project"
---

🚀 Project Progress and Future Plans

# 🚀 Project Progress and Future Plans

## 📅 Weekly Progress Sharing

Our team conducts weekly meetings to share progress among team members, ensuring everyone is aligned and up-to-date on the project's development.

## 🔧 Backend Development

### 🧑‍💻 User Service
- Implemented user signup and login functionality
- Developed data modification features
- Created trainer-user registration system

### 💪 Workout Service
- Added workout session management and exercise logging capabilities
- Implemented quest session feature for trainers to assign workouts to members

### 🛠 Technical Learnings
- Explored and implemented various features including:
  - JWT authentication
  - Asynchronous programming
  - Password hashing
  - Caching mechanisms
- Gained expertise in database design:
  - Normalization techniques
  - Schema design using PostgreSQL
  - Relational database management

## 🎨 Frontend Development

### 📐 UI/UX Design
- Utilized Figma for creating UI/UX designs
- Collaborated with team members to refine and improve designs
- Learned various UI/UX design tools and established a foundation in UI design principles

### 📱 Implementation Decision
After completing the UI design, we discussed implementation languages with the frontend team. After careful consideration of various factors, we decided to use Flutter for our frontend development.

#### Why Flutter? 🤔

1. **Cross-platform Development**: 
   Flutter allows us to deploy to both iOS and Android with a single codebase. This is crucial for rapidly collecting workout data to train our AI, as it enables us to reach a wider user base quickly.

2. **Customization Capabilities**: 
   While React Native was a strong contender due to its ability to use native widgets and notification styles, we prioritized full customization of our app. Flutter excels in providing extensive customization options from top to bottom.

3. **Performance**: 
   Flutter's compiled native code offers excellent performance, which is essential for smooth user interactions in a workout tracking app.

4. **Rich Widget Library**: 
   Flutter provides a comprehensive set of pre-built widgets that adhere to both Material Design and Cupertino styles, allowing for a consistent look across platforms while maintaining flexibility.

5. **Hot Reload**: 
   Flutter's hot reload feature significantly speeds up the development process, allowing us to see changes instantly without losing the app's state.

6. **Growing Community and Support**: 
   Flutter has a rapidly growing community and strong support from Google, ensuring long-term viability and continuous improvements.

7. **Dart Language**: 
   Dart, the programming language used in Flutter, is easy to learn for developers familiar with object-oriented concepts, making it accessible for our team.

8. **Integration with Firebase**: 
   Flutter's seamless integration with Firebase services aligns well with our backend needs and potential future scalability requirements.

While React Native offers its own set of advantages, Flutter's combination of performance, customization, and cross-platform capabilities made it the ideal choice for our project's specific needs and long-term goals.

## 📸 UI Screenshots

<table>
  <tr>
    <td><img src="/assets/posting/ui design 1.png" alt="Login Screen" width="400"/></td>
    <td><img src="/assets/posting/ui design 2.png" alt="Sign Up Screen" width="400"/></td>
    <td><img src="/assets/posting/ui design 3.png" alt="Workout Session Screen" width="400"/></td>
    <td><img src="/assets/posting/ui design 4.png" alt="Trainer Settings Screen" width="400"/></td>
  </tr>
</table>

## 🔮 Future Plans for Next Sprint

### Backend Enhancements
- Implement refresh token functionality for improved security
- Develop backend endpoints to support various graphs and statistical data to motivate users
- Explore advanced caching strategies for improved performance
- Establish integration tests for all microservices

### Frontend Development
- Gradually develop the frontend using Flutter, focusing on one screen at a time
- Create reusable widget components to maintain consistency across the app
- Implement responsive designs to ensure a great user experience across different device sizes
- Use Hero widget for widget animations and better user experience

### AI Integration
- Begin collecting and analyzing workout data to train AI models
- Develop AI-powered features for personalized workout plans

By focusing on these areas, we aim to create a robust, user-friendly, and innovative fitness app that stands out in the market and provides real value to both users and trainers.