# University Access Control System: Role-Based and Discretionary Access Control

## Overview

In this project, you will implement **Role-Based Access Control (RBAC)** and **Discretionary Access Control (DAC)** using Oso, a powerful authorization framework, in the context of a **university system**. The system will manage access for **Students**, **Professors**, and **Department Heads** to various university resources like courses, research projects, and grades.

### Key Concepts

- **RBAC (Role-Based Access Control)**: Users (students, professors, department heads) are assigned roles that determine what actions they can perform on university resources (courses, projects, etc.).
- **DAC (Discretionary Access Control)**: In this system, certain users, such as professors, can manage access to resources they own or oversee, such as research projects or course grades.

---

## Project Structure

### 1. **`app.py`**
   - The main script that manages user roles and permissions, enforcing policies for accessing courses, projects, and student grades.
   - Implements role assignments for Students, Professors, and the Department Head.

### 2. **`main.polar`**
   - The authorization policy file written in Polar (Oso’s policy language).
   - Contains RBAC rules that assign roles and permissions to users and DAC rules allowing professors to manage their own course resources.

### 3. **`models.py`**
   - Defines data models for users (students, professors, department head) and resources (courses, research projects, grades).
   - Includes logic for assigning roles and managing ownership of resources like courses and projects.

### 4. **`requirements.txt`**
   - A file that lists all the necessary dependencies for this project Oso.

---
### 1. Install Required Libraries

First, ensure Python 3.7+ is installed. Install Oso and any other required libraries by running:

```bash
pip install oso
```

### 2. Define Roles and Resources
In models.py, define the users and resources:

**Students** can enroll in courses, submit assignments, and view grades for the courses they are enrolled in. **Professors** can create courses, assign grades, and manage student access to their course materials and projects. The **Department Head** has oversight of all courses and research projects in the department, with permission to manage professors and their courses.
Resources include **Courses** (contain course materials, grades, and student enrollments) and **Research Projects** (professors can own and manage projects, and students can contribute to them).

### 3. Write the Oso Policy for RBAC
In main.polar, define the access control policy:

**Students** can access only the courses they are enrolled in. They may have read-only permissions for course materials and grades.
**Professors** can manage their courses, assign grades, and grant access to research projects.
**Department Head** can oversee all courses and projects in the department and manage both professors and students.

#### Hierarchy of Roles
The Department Head role is higher than both professors and students, allowing oversight and administrative control. Professors have authority over their courses and research projects but not others unless specifically granted permission by the department head.

### Note
You must use Polar to define rules for permissions (e.g., reading materials, submitting assignments, or assigning grades).
Assign roles such as student, professor, and department_head that map to specific actions on resources like courses and projects.

For example:



```polar
allow(actor, "view", course) if
    has_role(actor, "student", course) and
```

### Example Testing

def test_student_cannot_assign_grades():
    with pytest.raises(OsoNotAuthorizedError):
    
        oso.authorize(student, "assign_grade", course)
def test_student_access_unenrolled_course():
    with pytest.raises(OsoNotAuthorizedError):
        oso.authorize(student, "view", unenrolled_course)

