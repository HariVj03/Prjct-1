# Prjct-1
const express = require('express');
const router = express.Router();
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

// Secret key for JWT
const secretKey = 'your_secret_key';

// User model (for demo purposes)
const users = [];

// Sign-up route
router.post('/signup', (req, res) => {
  const { username, password } = req.body;
  const hashedPassword = bcrypt.hashSync(password, 10);
  const user = { username, password: hashedPassword };
  users.push(user);
  res.status(201).json({ message: 'User created successfully' });
});

// Sign-in route
router.post('/signin', (req, res) => {
  const { username, password } = req.body;
  const user = users.find((u) => u.username === username);
  if (!user) {
    res.status(401).json({ message: 'Invalid username or password' });
    return;
  }
  if (!bcrypt.compareSync(password, user.password)) {
    res.status(401).json({ message: 'Invalid username or password' });
    return;
  }
  const token = jwt.sign({ username: user.username }, secretKey);
  res.json({ token });
});

// Protected route (example)
router.get('/protected', (req, res) => {
  // Verify token here
  // ...
  res.json({ message: 'Protected resource' });
});

module.exports = router;
