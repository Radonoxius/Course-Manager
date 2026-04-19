# CMS Frontend — Course Management System

Frontend for Student Course Management System in React + Vite + Tailwind CSS.

---

## Tech Stack

| Tool | Use |
|---|---|
| React 18 | UI library |
| Vite | Build tool |
| React Router v6 | Client-side routing |
| Axios | HTTP requests |
| Tailwind CSS | Styling |

---

## Setup & Run

```bash
# 1. Install dependencies
npm install

# 2. .env file
cp .env.example .env

# 3. Dev server start
npm run dev
```

**.env file:**
```
VITE_API_URL=http://localhost:5000/api/v1
```

> Backend should be running at `http://localhost:5000`.

---

## File Structure

```
cms-frontend/
├── index.html
├── vite.config.js
├── tailwind.config.js
├── postcss.config.js
├── .env
├── package.json
│
└── src/
    ├── main.jsx               # React app entry point
    ├── App.jsx                # Routes + ProtectedRoute
    ├── index.css              # Tailwind directives
    │
    ├── api/                   # Axios API functions
    │   ├── axios.js           # Base instance + interceptors
    │   ├── auth.js            # login, signup, getMe
    │   ├── students.js        # CRUD
    │   ├── courses.js         # CRUD
    │   ├── departments.js     # CRUD
    │   ├── professors.js      # CRUD
    │   ├── semesters.js       # CRUD + enrollments by semester
    │   ├── enrollments.js     # CRUD
    │   └── grades.js          # assign, update, delete, filter
    │
    ├── context/
    │   └── AuthContext.jsx    # user, login, logout, isAdmin
    │
    ├── hooks/                 # Custom hooks (data + actions)
    │   ├── useCourses.js
    │   ├── useDepartments.js
    │   ├── useEnrollments.js
    │   ├── useGrades.js
    │   ├── useProfessors.js
    │   ├── useSemesters.js
    │   └── useToast.js
    │
    ├── components/
    │   ├── layout/
    │   │   ├── Layout.jsx     # Sidebar + Topbar + <Outlet>
    │   │   ├── Sidebar.jsx    # Nav links, role-based filter
    │   │   └── Topbar.jsx     # Page title, user info, logout
    │   │
    │   └── ui/
    │       ├── DataTable.jsx  # Search + table + edit/delete
    │       ├── Modal.jsx      # Reusable modal (isOpen prop)
    │       ├── Toast.jsx      # Success/error notification
    │       ├── StatCard.jsx   # Dashboard metric card
    │       ├── GradeBadge.jsx # Colored grade pill (A/B/C/D/F)
    │       └── Spinner.jsx    # Loading spinner
    │
    └── pages/
        ├── LoginPage.jsx      # Email + password login
        ├── SignupPage.jsx     # Name + email + password + role
        ├── Dashboard.jsx      # Stats + recent enrollments/grades
        │
        ├── students/
        │   ├── StudentsPage.jsx    # List + CRUD
        │   └── StudentForm.jsx     # Name, image, department dropdown
        │
        ├── courses/
        │   ├── CoursesPage.jsx
        │   └── CourseForm.jsx      # Name, credits, dept, professor
        │
        ├── departments/
        │   ├── DepartmentsPage.jsx
        │   └── DepartmentForm.jsx  # Name, building
        │
        ├── professors/
        │   ├── ProfessorsPage.jsx
        │   └── ProfessorForm.jsx   # Name, department dropdown
        │
        ├── semesters/
        │   ├── SemestersPage.jsx
        │   └── SemesterForm.jsx    # Name only
        │
        ├── enrollments/
        │   ├── EnrollmentsPage.jsx
        │   └── EnrollmentForm.jsx  # Student + Course + Semester dropdowns
        │
        └── grades/
            ├── GradesPage.jsx
            └── GradeForm.jsx       # Enrollment dropdown + A/B/C/D/F
```

---

## Routing

```
/login          → LoginPage       (public)
/signup         → SignupPage      (public)
/               → Dashboard       (protected)
/students       → StudentsPage    (protected)
/courses        → CoursesPage     (protected)
/departments    → DepartmentsPage (protected)
/professors     → ProfessorsPage  (protected)
/semesters      → SemestersPage   (protected)
/enrollments    → EnrollmentsPage (protected)
/grades         → GradesPage      (protected)
```

Protected routes — Redirect to `/login` if you dont have token.

---

## Auth Flow

```
User logs in
    ↓
POST /api/v1/auth/login
    ↓
token → saved at localStorage
    ↓
User set at AuthContext
    ↓
Dashboard redirect
```

**How token is attached:**  
`api/axios.js` request interceptor will automatically add `Authorization: Bearer <token>` header add karta hai.

**When reaching 401:**  
Redirect to `/logic` on token delete.

---

## API Layer

Every module is in `src/api/`:

```js
// example — students.js
export const getStudents = () => api.get("/students");
export const createStudent = (data) => api.post("/students", data);
export const updateStudent = (id, data) => api.put(`/students/${id}`, data);
export const deleteStudent = (id) => api.delete(`/students/${id}`);
```

**Important fixes:**
- Send `enrollment_id` to `parseInt()` — backend expects integer
- Backend response is wrapped — Use `res.data?.data || res.data`

---

## Custom Hooks

Module hook handles data fetch + CRUD actions:

```js
const { students, loading, error, addStudent, editStudent, removeStudent } = useStudents();
```

| Hook | Returns |
|---|---|
| `useCourses` | courses, addCourse, editCourse, removeCourse |
| `useDepartments` | departments, addDepartment, editDepartment, removeDepartment |
| `useEnrollments` | enrollments, addEnrollment, removeEnrollment |
| `useGrades` | grades, addGrade, editGrade, removeGrade |
| `useProfessors` | professors, addProfessor, editProfessor, removeProfessor |
| `useSemesters` | semesters, addSemester, editSemester, removeSemester |
| `useToast` | toast, success(), error(), clear() |

---

## Reusable Components

### `DataTable`
```jsx
<DataTable
  columns={COLUMNS}   // [{ key, label, render? }]
  data={data}         // array
  onEdit={openEdit}   // (row) => void
  onDelete={handleDelete} // (id) => void
/>
```

### `Modal`
```jsx
<Modal
  isOpen={showModal}  // boolean — required
  title="Add Student"
  onClose={closeModal}
>
  {/* form content */}
</Modal>
```

> Pass `isOpen` prop.

### `Toast`
```jsx
{toast && (
  <Toast
    message={toast.message}
    type={toast.type}        // "success" | "error"
    onClose={() => setToast(null)}
  />
)}
```

### `GradeBadge`
```jsx
<GradeBadge grade="A" />  // green pill
<GradeBadge grade="F" />  // red pill
```

---

## Role System

Choose `role: "student"` or `role: "admin"` on signup.

```js
// AuthContext se
const { isAdmin, isStudent } = useAuth();
```

- **Admin** — handles everything
- **Student** — limited access, can view enrollments and grades

---

## Known Issues & Fixes

| Issue | Fix |
|---|---|
| `data.filter is not a function` | Backend gives wrapped response — Use `res.data?.data \|\| res.data` |
| `departments.map is not a function` | Same — array extract first |
| `enrollment_id is not valid JSON` | `parseInt(enrollment_id)` before API call |
| Modal nahi khulta | `isOpen={showModal}` prop pass |
| Professor delete 500 error | Foreign key — `UPDATE courses SET professor_id = NULL` |
| `editX is not a function` | Missing hook — Update |

---

## Environment Variables

```
VITE_API_URL=http://localhost:5000/api/v1
```

---

## Scripts

```bash
npm run dev      # development server
npm run build    # production build
npm run preview  # preview build
```
