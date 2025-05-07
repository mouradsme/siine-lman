# Educational Dashboard System - Project Plan

## Project Overview

This project involves creating a comprehensive dashboard system for educational management with the following features:

- **User Roles**: Super Admin and Teacher accounts
- **Category Management**: Hierarchical categories with parent-child relationships
- **Subject Management**: Subjects belonging to multiple categories
- **Teacher Management**: Admin can assign multiple subjects to teachers
- **Learning Path Management**: Teachers can create curriculums for their subjects
- **Content Creation**: Teachers can create lessons and quizzes

## Technical Architecture

Based on the student's knowledge of C programming, HTML, CSS, JS, and some React, we recommend:

- **Frontend**: React.js with a component library (Material UI or Chakra UI)
- **Backend**: Node.js with Express (REST API)
- **Database**: MongoDB (for flexible schema) or PostgreSQL
- **Authentication**: JWT-based authentication
- **State Management**: Context API or Redux

## Project Phases

### Phase 1: Project Setup & Authentication

1. **Initial Setup**
   - Create React application
   - Set up project structure
   - Install necessary dependencies
   - Set up version control (Git)

2. **Authentication System**
   - Login/logout functionality
   - JWT token management
   - Password hashing
   - Role-based authorization
   - Protected routes

3. **Basic Layout**
   - Main dashboard layout
   - Navigation menu
   - Role-specific views

### Phase 2: Admin Dashboard

1. **Category Management**
   - Create/edit/delete categories
   - Set parent categories
   - Visualize category hierarchy
   - Drag-and-drop category organization

2. **Subject Management**
   - Create/edit/delete subjects
   - Assign subjects to categories
   - Search/filter subjects
   - Subject detail view

3. **Teacher Management**
   - Create teacher accounts
   - Manage teacher information
   - Assign/remove subjects from teachers
   - View teacher workload

### Phase 3: Teacher Dashboard

1. **Learning Path Management**
   - Create/edit/delete learning paths for assigned subjects
   - Version control for learning path editions
   - Visualize learning path structure
   - Organize content within paths

2. **Lesson Management**
   - Create/edit/delete lessons
   - Rich text editor for lesson content
   - Media embedding (images, videos)
   - Lesson preview functionality

3. **Quiz Management**
   - Create/edit/delete quizzes
   - Multiple question types:
     - Multiple choice
     - True/False
     - Short answer
     - Matching
   - Answer validation logic
   - Quiz settings (time limits, attempts)

### Phase 4: Data Relationships & Advanced Features

1. **Implement Data Relationships**
   - Categories to subcategories (one-to-many)
   - Categories to subjects (many-to-many)
   - Subjects to teachers (many-to-many)
   - Subjects to learning paths (one-to-many)
   - Learning paths to lessons/quizzes (one-to-many)

2. **Advanced Features**
   - Dashboard analytics
   - Batch operations
   - Search functionality
   - Content previews
   - Export functionality
   - Activity logs

## Database Schema Design

### Users Collection
```
{
  _id: ObjectId,
  name: String,
  email: String,
  password: String (hashed),
  role: String (enum: ["admin", "teacher"]),
  createdAt: Date,
  updatedAt: Date
}
```

### Categories Collection
```
{
  _id: ObjectId,
  name: String,
  description: String,
  parentCategoryId: ObjectId (reference to Categories),
  createdAt: Date,
  updatedAt: Date
}
```

### Subjects Collection
```
{
  _id: ObjectId,
  name: String,
  description: String,
  categoryIds: [ObjectId] (references to Categories),
  createdAt: Date,
  updatedAt: Date
}
```

### TeacherSubjects Collection
```
{
  _id: ObjectId,
  teacherId: ObjectId (reference to Users),
  subjectId: ObjectId (reference to Subjects),
  assignedAt: Date
}
```

### LearningPaths Collection
```
{
  _id: ObjectId,
  name: String,
  description: String,
  subjectId: ObjectId (reference to Subjects),
  teacherId: ObjectId (reference to Users),
  version: String,
  status: String (enum: ["draft", "published"]),
  createdAt: Date,
  updatedAt: Date
}
```

### Lessons Collection
```
{
  _id: ObjectId,
  title: String,
  content: String (rich text),
  learningPathId: ObjectId (reference to LearningPaths),
  order: Number,
  createdAt: Date,
  updatedAt: Date
}
```

### Quizzes Collection
```
{
  _id: ObjectId,
  title: String,
  description: String,
  learningPathId: ObjectId (reference to LearningPaths),
  timeLimit: Number (minutes),
  passingScore: Number,
  createdAt: Date,
  updatedAt: Date
}
```

### Questions Collection
```
{
  _id: ObjectId,
  quizId: ObjectId (reference to Quizzes),
  questionText: String,
  questionType: String (enum: ["multiple-choice", "true-false", "short-answer"]),
  points: Number,
  order: Number,
  createdAt: Date,
  updatedAt: Date
}
```

### Answers Collection
```
{
  _id: ObjectId,
  questionId: ObjectId (reference to Questions),
  answerText: String,
  isCorrect: Boolean,
  order: Number
}
```

## API Endpoints

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/profile` - Get current user profile

### Categories (Admin only)
- `GET /api/categories` - Get all categories
- `POST /api/categories` - Create category
- `GET /api/categories/:id` - Get category by ID
- `PUT /api/categories/:id` - Update category
- `DELETE /api/categories/:id` - Delete category

### Subjects (Admin only)
- `GET /api/subjects` - Get all subjects
- `POST /api/subjects` - Create subject
- `GET /api/subjects/:id` - Get subject by ID
- `PUT /api/subjects/:id` - Update subject
- `DELETE /api/subjects/:id` - Delete subject

### Teachers (Admin only)
- `GET /api/teachers` - Get all teachers
- `POST /api/teachers` - Create teacher account
- `GET /api/teachers/:id` - Get teacher by ID
- `PUT /api/teachers/:id` - Update teacher
- `DELETE /api/teachers/:id` - Delete teacher
- `POST /api/teachers/:id/subjects` - Assign subject to teacher
- `DELETE /api/teachers/:id/subjects/:subjectId` - Remove subject from teacher

### Learning Paths (Teacher)
- `GET /api/learning-paths` - Get all learning paths (for teacher's subjects)
- `POST /api/learning-paths` - Create learning path
- `GET /api/learning-paths/:id` - Get learning path by ID
- `PUT /api/learning-paths/:id` - Update learning path
- `DELETE /api/learning-paths/:id` - Delete learning path

### Lessons (Teacher)
- `GET /api/learning-paths/:pathId/lessons` - Get all lessons for a learning path
- `POST /api/learning-paths/:pathId/lessons` - Create lesson
- `GET /api/lessons/:id` - Get lesson by ID
- `PUT /api/lessons/:id` - Update lesson
- `DELETE /api/lessons/:id` - Delete lesson

### Quizzes (Teacher)
- `GET /api/learning-paths/:pathId/quizzes` - Get all quizzes for a learning path
- `POST /api/learning-paths/:pathId/quizzes` - Create quiz
- `GET /api/quizzes/:id` - Get quiz by ID
- `PUT /api/quizzes/:id` - Update quiz
- `DELETE /api/quizzes/:id` - Delete quiz

### Questions (Teacher)
- `GET /api/quizzes/:quizId/questions` - Get all questions for a quiz
- `POST /api/quizzes/:quizId/questions` - Create question
- `GET /api/questions/:id` - Get question by ID
- `PUT /api/questions/:id` - Update question
- `DELETE /api/questions/:id` - Delete question

## Implementation Tips

1. **Start Small**
   - Begin with core functionality and expand
   - Implement one feature at a time
   - Test thoroughly before moving on

2. **Component Design**
   - Use reusable components
   - Follow a consistent design system
   - Consider responsive design from the start

3. **State Management**
   - Use Context API for simple state management
   - Consider Redux for more complex state needs
   - Organize state logically by feature

4. **Authentication & Security**
   - Implement proper validation
   - Use HTTP-only cookies for JWT
   - Apply role-based access control consistently

5. **Testing**
   - Write unit tests for components
   - Test API endpoints
   - Perform usability testing

6. **Code Organization**
   - Follow a consistent folder structure
   - Use clear naming conventions
   - Document code where necessary

## Development Workflow

1. **Planning**
   - Define requirements for each feature
   - Create wireframes/mockups
   - Plan database schema

2. **Development**
   - Set up project structure
   - Implement core functionality
   - Add features incrementally

3. **Testing**
   - Test each feature thoroughly
   - Fix bugs as they arise
   - Get feedback from stakeholders

4. **Deployment**
   - Set up deployment environment
   - Deploy application
   - Monitor for issues

## Resources

### React Libraries
- Material UI: https://mui.com/
- Chakra UI: https://chakra-ui.com/
- React Router: https://reactrouter.com/
- Formik: https://formik.org/
- React Query: https://react-query.tanstack.com/

### Backend Resources
- Express.js: https://expressjs.com/
- Mongoose (MongoDB): https://mongoosejs.com/
- JWT: https://jwt.io/
- Bcrypt: https://www.npmjs.com/package/bcrypt

### Learning Resources
- React Documentation: https://reactjs.org/docs/getting-started.html
- Node.js Documentation: https://nodejs.org/en/docs/
- MongoDB Documentation: https://docs.mongodb.com/
- JWT Authentication Tutorial: https://jwt.io/introduction/
