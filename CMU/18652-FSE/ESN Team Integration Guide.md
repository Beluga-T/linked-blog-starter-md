

*Complete Specification for Join Community, Login/Logout & Share Status Use Cases*

  

**Team:** MengChao (Database) | Siwei (Services/Utilities) | Lexie (Controllers/Routes) | Sammy (Frontend API) | Joyce (Frontend UI)

  

---

  

## 🚀 **Quick Start - What Each Person Implements**

  

### **MengChao** - Database

```javascript

// File: models/User.js - User data model with password hashing

  

KEY FUNCTIONS:

- User.findByUsername() → case-insensitive lookup

- user.comparePassword() → bcrypt validation

```

  

### **Siwei** - Business Logic & Utilities

```javascript

// File: services/authService.js - ALL authentication decisions

// File: utilities/jwtUtil.js - Pure JWT functions (generate/verify/extract)

  

KEY FUNCTIONS:

- handleJoinCommunity() → determines login/register/error

- createNewUser() → handles new user creation

- isBannedUsername() → business rule validation

- jwtUtil.generateToken() → creates JWT

- jwtUtil.verifyToken() → validates JWT

- updateEmergencyStatus() → updates user emergency status

- getESNDirectory() → returns users with status info

```

  

### **Lexie** - HTTP Layer

```javascript

// File: controllers/authController.js - HTTP request handling

// File: middleware/authentication.js - JWT verification for protected routes

// File: routes/auth.js - Route definitions with middleware

  

KEY FUNCTIONS:

- joinCommunity() → POST /api/auth/join-community

- authenticateToken() → JWT middleware for protected routes

```

  

### **Sammy** - Frontend API

```javascript

// File: public/js/auth.js - API calls and token management

// File: public/js/login.js - Login page JavaScript

  

KEY FUNCTIONS:

- handleJoinFlow() → manages entire authentication flow

- saveToken()/getToken() → localStorage token management

```

  

### **Joyce** - User Interface

```html

<!-- File: public/login.html - Single authentication form -->

<!-- File: public/css/style.css - Responsive styling -->

  

KEY ELEMENTS:

- Single "Join Community" form (no separate login/register)

- Confirmation modal for new users

- Welcome modal with status explanation

- Status selection interface (OK/Help/Emergency)

- Real-time status updates in directory

```

  

---

  

## 🎯 **Layer Responsibilities - WHO Does WHAT**

  

### **🗄️ MengChao - Database Layer**

**✅ YOU HANDLE:**

- Database schema and operations

- Password hashing with bcrypt

- Case-insensitive username lookups

- Data model methods

  

**❌ YOU DON'T:**

- Make authentication decisions

- Handle HTTP requests/responses

- Generate or verify JWT tokens

- Contain business rules

  

### **⚙️ Siwei - Service & Utilities Layer**

**✅ YOU HANDLE:**

- ALL authentication business logic

- Username/password validation rules

- User existence checking

- JWT utility functions (generate/verify/extract)

- Token generation after successful authentication

- All business decisions (login vs register vs error)

  

**❌ YOU DON'T:**

- Handle HTTP status codes

- Parse JWT tokens from HTTP requests (middleware does this)

- Deal with HTTP headers directly

- Handle frontend concerns

  

### **🔗 Lexie - HTTP Layer**

**✅ YOU HANDLE:**

- HTTP request/response handling

- Mapping service responses to status codes

- JWT token verification middleware

- Basic input format validation

- Route definitions and middleware application

  

**❌ YOU DON'T:**

- Generate JWT tokens

- Make authentication decisions

- Query database directly

- Contain business logic

  

### **🌐 Sammy - Frontend API Layer**

**✅ YOU HANDLE:**

- API calls to backend endpoints

- Token storage in localStorage

- Frontend authentication flow management

- Error handling and user feedback

- Modal state management

  

**❌ YOU DON'T:**

- Generate or verify JWT tokens

- Hash passwords

- Make authentication decisions

- Validate business rules

  

### **🎨 Joyce - UI Layer**

**✅ YOU HANDLE:**

- HTML structure and form elements

- CSS styling and responsive design

- Bootstrap integration

- Element IDs for JavaScript integration

- User experience and accessibility

  

**❌ YOU DON'T:**

- Include authentication logic in HTML

- Make API calls

- Handle tokens or passwords

- Contain business validation

  

---

  
  
  
  

## 🔄 **Complete Authentication Flow**

  

### **Step-by-Step Process:**

  

1. **User Action**: Fills form, clicks "Join Community"

2. **Frontend (Sammy)**: `handleJoinFlow()` → POST `/api/auth/join-community`

3. **Middleware (Lexie)**: `validateJoinCommunity()` → basic format validation

4. **Controller (Lexie)**: `joinCommunity()` → calls service layer

5. **Service (Siwei)**: `handleJoinCommunity()` → business logic and authentication

6. **Model (MengChao)**: `User.findByUsername()` + `comparePassword()`

7. **Utility (MengChao)**: `jwtUtil.generateToken()` if authentication succeeds

8. **Response Flow**: Service → Controller → Frontend

9. **Frontend (Sammy)**: Handle response → save token or show modals

10. **UI (Joyce)**: Display results → redirect or show errors

  

### **Three Possible Outcomes:**

  

**A) Existing User Login:**

- User exists + correct password → Generate token → Redirect to app

  

**B) New User Registration:**

- User doesn't exist → Show confirmation → Create account → Show welcome → Redirect

  

**C) Error Handling:**

- Format errors, wrong password, or validation failures → Show error message

  

---

  

## 🛠️ **Troubleshooting Common Issues**

  

### **Problem: Token Generation Not Working**

**Check:**

1. Service calling `jwtUtil.generateToken()` after authentication success?

2. `JWT_SECRET` environment variable set?

3. Controller NOT calling `jwtUtil` directly?

  

**Fix:** Move token generation to service layer only.

  

### **Problem: Authentication Always Fails**

**Check:**

1. All authentication logic in service layer?

2. Password comparison using `user.comparePassword()`?

3. Username lookup case-insensitive?

  

**Fix:** Consolidate authentication in service layer.

  

### **Problem: Protected Routes Return 401**

**Check:**

1. `authenticateToken` middleware applied to routes?

2. Frontend sending `Authorization: Bearer <token>` header?

3. Middleware only verifying tokens, not making business decisions?

  

**Fix:** Apply middleware correctly and check token format.

  

### **Problem: Business Logic Scattered**

**Check:**

1. Username validation only in service layer?

2. Controllers only handling HTTP status mapping?

3. Middleware only doing technical verification?

  

**Fix:** Move all business logic to service layer.

  

---

  

## ✅ **Testing Checklist**

  

### **For Each Team Member:**

  

**MengChao:**

- [ ] User model saves with hashed passwords

- [ ] `findByUsername()` works case-insensitively

- [ ] `comparePassword()` validates correctly

- [ ] Database schema and operations work correctly

  

**Siwei:**

- [ ] `handleJoinCommunity()` handles all 3 outcomes

- [ ] Business validation (3+ chars, banned usernames) works

- [ ] Service generates tokens after successful authentication

- [ ] JWT utilities are pure functions (no business logic)

- [ ] No HTTP status codes in service responses

  

**Lexie:**

- [ ] `/api/auth/join-community` endpoint works

- [ ] Middleware validates basic input format

- [ ] `authenticateToken` middleware protects routes

- [ ] Controllers only handle HTTP, no business logic

  

**Sammy:**

- [ ] `handleJoinFlow()` manages complete user experience

- [ ] Token storage/retrieval from localStorage works

- [ ] API calls include Authorization headers for protected routes

- [ ] Error handling for all response types

  

**Joyce:**

- [ ] Single form with "Join Community" button

- [ ] Confirmation and welcome modals work

- [ ] Responsive design on mobile devices

- [ ] All required element IDs present

  

### **Integration Tests:**

- [ ] Username too short → format error

- [ ] Existing user + correct password → login + redirect

- [ ] Existing user + wrong password → error message

- [ ] New username → confirmation modal → account creation

- [ ] JWT token works for protected routes

- [ ] Welcome message shows for new users only

  

---

  

## 📋 **Quick Reference**

  

### **File Structure:**

```

models/User.js (MengChao)

utilities/jwtUtil.js (Siwei)

services/authService.js (Siwei)

controllers/authController.js (Lexie)

middleware/authentication.js (Lexie)

middleware/validation.js (Lexie)

routes/auth.js (Lexie)

public/js/auth.js (Sammy)

public/js/login.js (Sammy)

public/login.html (Joyce)

public/css/style.css (Joyce)

```

  

### **Key Environment Variables:**

```

JWT_SECRET=your-secret-key

MONGODB_URI=mongodb://localhost:27017/esn

```

  

### **Development Commands:**

```bash

npm start # Production

npm run dev # Development with nodemon

```

  

---

  

*This single guide contains everything your team needs for seamless integration of the Join Community and Login/Logout use cases.*