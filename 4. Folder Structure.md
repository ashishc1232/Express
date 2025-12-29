
## 1. Project Folder Structure

```
express-app/
│
├── server.js
├── package.json
│
├── routes/
│   └── userRoutes.js
│
├── controllers/
│   └── userController.js
│
├── models/
│   └── userModel.js
│
├── middleware/
│   └── authMiddleware.js
│
└── config/
    └── db.js
```


## 2. server.js (Entry Point)

```js
const express = require('express');
const app = express();

const userRoutes = require('./routes/userRoutes');

// middleware to parse JSON
app.use(express.json());

// routes
app.use('/api/users', userRoutes);

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

## 3. routes/userRoutes.js

**Only handles URL + HTTP method mapping**

```js
const express = require('express');
const router = express.Router();

const {
  getUsers,
  getUserById,
  createUser
} = require('../controllers/userController');

const authMiddleware = require('../middleware/authMiddleware');

// GET all users
router.get('/', authMiddleware, getUsers);

// GET user by id
router.get('/:id', getUserById);

// CREATE user
router.post('/', createUser);

module.exports = router;
```

---

## 4. controllers/userController.js

**Business logic stays here**

```js
const User = require('../models/userModel');

// GET /api/users
const getUsers = (req, res) => {
  const users = User.getAllUsers();
  res.status(200).json(users);
};

// GET /api/users/:id
const getUserById = (req, res) => {
  const user = User.getUserById(req.params.id);

  if (!user) {
    return res.status(404).json({ message: 'User not found' });
  }

  res.status(200).json(user);
};

// POST /api/users
const createUser = (req, res) => {
  const { name, email } = req.body;

  const newUser = User.createUser(name, email);

  res.status(201).json(newUser);
};

module.exports = {
  getUsers,
  getUserById,
  createUser
};
```

---

## 5. models/userModel.js

**Handles data (DB / fake data / logic related to storage)**

```js
let users = [
  { id: 1, name: 'Amit', email: 'amit@gmail.com' },
  { id: 2, name: 'Ravi', email: 'ravi@gmail.com' }
];

const getAllUsers = () => {
  return users;
};

const getUserById = (id) => {
  return users.find(user => user.id === Number(id));
};

const createUser = (name, email) => {
  const newUser = {
    id: users.length + 1,
    name,
    email
  };

  users.push(newUser);
  return newUser;
};

module.exports = {
  getAllUsers,
  getUserById,
  createUser
};
```

---

## 6. middleware/authMiddleware.js

**Runs before controller (validation, auth, logging, etc.)**

```js
const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ message: 'Unauthorized' });
  }

  // assume token is valid
  next();
};

module.exports = authMiddleware;
```

---

## 7. Flow (Very Important)

```
Request
 ↓
server.js
 ↓
routes
 ↓
middleware (optional)
 ↓
controller
 ↓
model
 ↓
response
```

---

## 8. Why This Structure?

| Folder      | Responsibility            |
| ----------- | ------------------------- |
| routes      | URL + HTTP method         |
| controllers | Business logic            |
| models      | Data handling             |
| middleware  | Auth, validation, logging |
| server.js   | App entry point           |

---

## 9. Example API Calls

```
GET  /api/users
GET  /api/users/1
POST /api/users
```

POST body:

```json
{
  "name": "Suresh",
  "email": "suresh@gmail.com"
}
```


