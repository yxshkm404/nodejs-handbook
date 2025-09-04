# Node.js Require & Export 🔄

Welcome to the complete guide on Node.js require and export! This is your gateway to understanding how modules communicate with each other in Node.js - think of it as learning the **language** that your code files use to talk to each other.

## What are Require & Export? 🤔

Imagine you're building a house 🏠. You don't create every brick, window, and door from scratch. Instead, you **import** pre-made components and **export** your finished rooms for others to use.

That's exactly what `require` and `export` do in Node.js:

- **`require()`** = **"Hey, I need this tool/function from another file!"** 📥
- **`module.exports`** = **"Here's what I'm sharing with the world!"** 📤

![Require Export Concept](https://via.placeholder.com/600x300/4CAF50/FFFFFF?text=require%28%29+%3D+Import+%F0%9F%93%A5+%7C+module.exports+%3D+Share+%F0%9F%93%A4)

> **Real-world analogy:** Think of `require` as **borrowing** tools from a friend, and `export` as **lending** your tools to others. You specify what you're willing to lend, and others can borrow exactly what they need!

## The Basics: Your First Export 📤

Let's start with the simplest example - creating a function and sharing it:

### Step 1: Create a Helper Module

```javascript
// math-helper.js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

// Share these functions with other files
module.exports = {
    add: add,
    subtract: subtract
};

// Or using ES6 shorthand (same result)
// module.exports = { add, subtract };
```

### Step 2: Use the Helper Module

```javascript
// main.js
const mathHelper = require('./math-helper');

console.log(mathHelper.add(5, 3));      // Output: 8
console.log(mathHelper.subtract(10, 4)); // Output: 6

// You can also destructure what you need
const { add, subtract } = require('./math-helper');
console.log(add(2, 3));     // Output: 5
console.log(subtract(9, 4)); // Output: 5
```

**🎉 Congratulations!** You just created your first module communication!

## Different Export Patterns 🎨

### 1. Object Export (Most Common)

```javascript
// user-service.js
function createUser(name, email) {
    return {
        id: Date.now(),
        name: name,
        email: email,
        createdAt: new Date()
    };
}

function validateEmail(email) {
    return email.includes('@') && email.includes('.');
}

function formatUserName(name) {
    return name.trim().toLowerCase()
        .split(' ')
        .map(word => word.charAt(0).toUpperCase() + word.slice(1))
        .join(' ');
}

// Export multiple items as an object
module.exports = {
    createUser,
    validateEmail,
    formatUserName
};
```

```javascript
// app.js
const userService = require('./user-service');

// Use the exported functions
const newUser = userService.createUser('john doe', 'john@example.com');
console.log(newUser);

const isValid = userService.validateEmail('test@gmail.com');
console.log('Email valid:', isValid);

const formatted = userService.formatUserName('  alice  smith  ');
console.log('Formatted name:', formatted); // "Alice Smith"
```

### 2. Single Function Export

```javascript
// logger.js
function log(message, level = 'INFO') {
    const timestamp = new Date().toISOString();
    const emoji = level === 'ERROR' ? '❌' : level === 'WARN' ? '⚠️' : '✅';
    
    console.log(`[${timestamp}] ${emoji} ${level}: ${message}`);
}

// Export single function directly
module.exports = log;
```

```javascript
// server.js
const log = require('./logger');

log('Server starting...'); 
log('Database connected!');
log('Something went wrong!', 'ERROR');
log('Memory usage high', 'WARN');
```

**Output:**
```
[2024-01-15T10:30:45.123Z] ✅ INFO: Server starting...
[2024-01-15T10:30:45.124Z] ✅ INFO: Database connected!
[2024-01-15T10:30:45.125Z] ❌ ERROR: Something went wrong!
[2024-01-15T10:30:45.126Z] ⚠️ WARN: Memory usage high
```

### 3. Class Export

```javascript
// task-manager.js
class TaskManager {
    constructor() {
        this.tasks = [];
        this.nextId = 1;
    }
    
    addTask(title, description = '') {
        const task = {
            id: this.nextId++,
            title,
            description,
            completed: false,
            createdAt: new Date()
        };
        
        this.tasks.push(task);
        console.log(`✅ Task added: "${title}"`);
        return task;
    }
    
    completeTask(id) {
        const task = this.tasks.find(t => t.id === id);
        if (task) {
            task.completed = true;
            task.completedAt = new Date();
            console.log(`🎉 Task completed: "${task.title}"`);
            return task;
        }
        console.log(`❌ Task with ID ${id} not found`);
        return null;
    }
    
    getTasks(showCompleted = true) {
        if (showCompleted) {
            return this.tasks;
        }
        return this.tasks.filter(task => !task.completed);
    }
    
    getTaskCount() {
        const total = this.tasks.length;
        const completed = this.tasks.filter(t => t.completed).length;
        return { total, completed, pending: total - completed };
    }
}

// Export the class
module.exports = TaskManager;
```

```javascript
// todo-app.js
const TaskManager = require('./task-manager');

// Create new instance
const myTasks = new TaskManager();

// Add some tasks
myTasks.addTask('Learn Node.js', 'Complete the require/export tutorial');
myTasks.addTask('Build a project', 'Create a simple Node.js application');
myTasks.addTask('Deploy app', 'Deploy to production server');

// Complete a task
myTasks.completeTask(1);

// Get task statistics
const stats = myTasks.getTaskCount();
console.log('📊 Task Statistics:', stats);

// Get all tasks
const allTasks = myTasks.getTasks();
console.log('📋 All Tasks:', allTasks);
```

### 4. Mixed Export Patterns

```javascript
// config-manager.js
const DEFAULT_CONFIG = {
    port: 3000,
    database: {
        host: 'localhost',
        port: 5432,
        name: 'myapp'
    },
    logging: {
        level: 'info',
        file: 'app.log'
    }
};

class ConfigManager {
    constructor() {
        this.config = { ...DEFAULT_CONFIG };
    }
    
    get(path) {
        return path.split('.').reduce((obj, key) => obj && obj[key], this.config);
    }
    
    set(path, value) {
        const keys = path.split('.');
        const lastKey = keys.pop();
        const target = keys.reduce((obj, key) => obj[key] = obj[key] || {}, this.config);
        target[lastKey] = value;
    }
}

function createConfig() {
    return new ConfigManager();
}

// Export multiple types
module.exports = {
    ConfigManager,      // Class
    createConfig,       // Function
    DEFAULT_CONFIG,     // Object
    version: '1.0.0'   // Simple value
};
```

```javascript
// app-config.js
const { ConfigManager, createConfig, DEFAULT_CONFIG, version } = require('./config-manager');

console.log('Config version:', version);
console.log('Default config:', DEFAULT_CONFIG);

// Use the factory function
const config = createConfig();
config.set('port', 8080);
config.set('database.host', 'production-db');

console.log('Port:', config.get('port'));
console.log('DB Host:', config.get('database.host'));

// Or use the class directly
const anotherConfig = new ConfigManager();
console.log('Another config port:', anotherConfig.get('port'));
```

## Advanced Export Techniques 🚀

### 1. Immediate Export

```javascript
// utilities.js

// Export functions directly without storing in variables
module.exports.randomId = function() {
    return Math.random().toString(36).substr(2, 9);
};

module.exports.formatDate = function(date) {
    return date.toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
    });
};

module.exports.capitalize = function(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
};

// You can also mix with object exports
module.exports.constants = {
    MAX_RETRIES: 3,
    TIMEOUT: 5000,
    API_VERSION: 'v1'
};
```

### 2. Conditional Exports

```javascript
// environment-config.js
const isDevelopment = process.env.NODE_ENV === 'development';
const isProduction = process.env.NODE_ENV === 'production';

let config;

if (isDevelopment) {
    config = {
        apiUrl: 'http://localhost:3001',
        debug: true,
        logLevel: 'debug',
        database: 'dev_database'
    };
} else if (isProduction) {
    config = {
        apiUrl: 'https://api.myapp.com',
        debug: false,
        logLevel: 'error',
        database: 'prod_database'
    };
} else {
    config = {
        apiUrl: 'http://test-api.myapp.com',
        debug: true,
        logLevel: 'info',
        database: 'test_database'
    };
}

// Export based on environment
module.exports = config;
```

### 3. Function Factory Pattern

```javascript
// database-connector.js
function createDatabaseConnection(options = {}) {
    const defaultOptions = {
        host: 'localhost',
        port: 5432,
        maxConnections: 10,
        timeout: 5000
    };
    
    const config = { ...defaultOptions, ...options };
    
    return {
        connect() {
            console.log(`🔌 Connecting to ${config.host}:${config.port}...`);
            return new Promise(resolve => {
                setTimeout(() => {
                    console.log('✅ Database connected!');
                    resolve({ status: 'connected', config });
                }, 1000);
            });
        },
        
        disconnect() {
            console.log('🔌 Disconnecting from database...');
            return Promise.resolve({ status: 'disconnected' });
        },
        
        query(sql) {
            console.log(`📝 Executing query: ${sql}`);
            return Promise.resolve({ results: `Results for: ${sql}` });
        }
    };
}

// Export factory function
module.exports = createDatabaseConnection;
```

```javascript
// database-usage.js
const createDB = require('./database-connector');

async function setupDatabase() {
    // Create development database connection
    const devDB = createDB({
        host: 'dev-server',
        port: 5432
    });
    
    // Create production database connection
    const prodDB = createDB({
        host: 'prod-server',
        port: 5432,
        maxConnections: 50
    });
    
    // Use the connections
    await devDB.connect();
    const result = await devDB.query('SELECT * FROM users');
    console.log(result);
    
    await devDB.disconnect();
}

setupDatabase();
```

## Understanding Require Paths 📍

### 1. Relative Paths

```javascript
// Current directory
const helper = require('./helper');
const utils = require('./utils');

// Parent directory
const config = require('../config');
const shared = require('../shared/utils');

// Nested directories
const userModel = require('./models/user');
const apiRoutes = require('./routes/api');
```

**Directory Structure Example:**
```
project/
├── app.js
├── config.js
├── helper.js
├── models/
│   └── user.js
├── routes/
│   └── api.js
└── shared/
    └── utils.js
```

```javascript
// app.js
const config = require('./config');           // Same directory
const userModel = require('./models/user');   // Subdirectory
const sharedUtils = require('./shared/utils'); // Subdirectory

// models/user.js
const config = require('../config');          // Parent directory
const sharedUtils = require('../shared/utils'); // Sibling directory
```

### 2. Absolute Paths (Node Modules)

```javascript
// Built-in Node.js modules
const fs = require('fs');
const path = require('path');
const http = require('http');

// Third-party modules from node_modules
const express = require('express');
const lodash = require('lodash');
const moment = require('moment');
```

### 3. Index.js Convention

Node.js automatically looks for `index.js` when you require a directory:

```
utils/
├── index.js
├── string-helpers.js
├── date-helpers.js
└── math-helpers.js
```

```javascript
// utils/index.js
const stringHelpers = require('./string-helpers');
const dateHelpers = require('./date-helpers');
const mathHelpers = require('./math-helpers');

// Re-export everything from one place
module.exports = {
    ...stringHelpers,
    ...dateHelpers,
    ...mathHelpers
};
```

```javascript
// app.js
const utils = require('./utils'); // Automatically loads utils/index.js

console.log(utils.formatDate(new Date()));
console.log(utils.add(5, 3));
console.log(utils.capitalize('hello'));
```

## Real-World Project Example 🌍

Let's build a complete mini-project to see require/export in action:

### Project Structure
```
blog-system/
├── app.js
├── models/
│   ├── index.js
│   ├── post.js
│   └── user.js
├── services/
│   ├── index.js
│   ├── blog-service.js
│   └── user-service.js
└── utils/
    ├── index.js
    ├── logger.js
    └── validator.js
```

### 1. Utility Modules

```javascript
// utils/logger.js
class Logger {
    static info(message) {
        console.log(`ℹ️  [INFO] ${new Date().toISOString()}: ${message}`);
    }
    
    static error(message) {
        console.log(`❌ [ERROR] ${new Date().toISOString()}: ${message}`);
    }
    
    static success(message) {
        console.log(`✅ [SUCCESS] ${new Date().toISOString()}: ${message}`);
    }
}

module.exports = Logger;
```

```javascript
// utils/validator.js
class Validator {
    static isValidEmail(email) {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(email);
    }
    
    static isValidPassword(password) {
        return password && password.length >= 6;
    }
    
    static isValidTitle(title) {
        return title && title.trim().length >= 5;
    }
}

module.exports = Validator;
```

```javascript
// utils/index.js
const Logger = require('./logger');
const Validator = require('./validator');

module.exports = {
    Logger,
    Validator
};
```

### 2. Model Layer

```javascript
// models/user.js
class User {
    constructor(name, email, password) {
        this.id = Date.now() + Math.random();
        this.name = name;
        this.email = email;
        this.password = password; // In real app, this would be hashed
        this.createdAt = new Date();
        this.posts = [];
    }
    
    addPost(post) {
        this.posts.push(post);
    }
    
    toJSON() {
        return {
            id: this.id,
            name: this.name,
            email: this.email,
            createdAt: this.createdAt,
            postCount: this.posts.length
        };
    }
}

module.exports = User;
```

```javascript
// models/post.js
class Post {
    constructor(title, content, authorId) {
        this.id = Date.now() + Math.random();
        this.title = title;
        this.content = content;
        this.authorId = authorId;
        this.createdAt = new Date();
        this.views = 0;
    }
    
    incrementViews() {
        this.views++;
    }
    
    toJSON() {
        return {
            id: this.id,
            title: this.title,
            content: this.content,
            authorId: this.authorId,
            createdAt: this.createdAt,
            views: this.views
        };
    }
}

module.exports = Post;
```

```javascript
// models/index.js
const User = require('./user');
const Post = require('./post');

module.exports = {
    User,
    Post
};
```

### 3. Service Layer

```javascript
// services/user-service.js
const { User } = require('../models');
const { Logger, Validator } = require('../utils');

class UserService {
    constructor() {
        this.users = new Map();
    }
    
    createUser(name, email, password) {
        Logger.info(`Attempting to create user: ${email}`);
        
        if (!Validator.isValidEmail(email)) {
            throw new Error('Invalid email format');
        }
        
        if (!Validator.isValidPassword(password)) {
            throw new Error('Password must be at least 6 characters');
        }
        
        if (this.findUserByEmail(email)) {
            throw new Error('User already exists');
        }
        
        const user = new User(name, email, password);
        this.users.set(user.id, user);
        
        Logger.success(`User created: ${user.email}`);
        return user;
    }
    
    findUserById(id) {
        return this.users.get(id);
    }
    
    findUserByEmail(email) {
        return Array.from(this.users.values())
            .find(user => user.email === email);
    }
    
    getAllUsers() {
        return Array.from(this.users.values())
            .map(user => user.toJSON());
    }
}

module.exports = new UserService(); // Export singleton instance
```

```javascript
// services/blog-service.js
const { Post } = require('../models');
const { Logger, Validator } = require('../utils');
const userService = require('./user-service');

class BlogService {
    constructor() {
        this.posts = new Map();
    }
    
    createPost(title, content, authorId) {
        Logger.info(`Creating post: ${title}`);
        
        if (!Validator.isValidTitle(title)) {
            throw new Error('Title must be at least 5 characters');
        }
        
        const author = userService.findUserById(authorId);
        if (!author) {
            throw new Error('Author not found');
        }
        
        const post = new Post(title, content, authorId);
        this.posts.set(post.id, post);
        author.addPost(post);
        
        Logger.success(`Post created: ${title}`);
        return post;
    }
    
    getPost(id) {
        const post = this.posts.get(id);
        if (post) {
            post.incrementViews();
        }
        return post;
    }
    
    getAllPosts() {
        return Array.from(this.posts.values())
            .map(post => ({
                ...post.toJSON(),
                author: userService.findUserById(post.authorId)?.toJSON()
            }));
    }
    
    getPostsByAuthor(authorId) {
        return Array.from(this.posts.values())
            .filter(post => post.authorId === authorId)
            .map(post => post.toJSON());
    }
}

module.exports = new BlogService(); // Export singleton instance
```

```javascript
// services/index.js
const userService = require('./user-service');
const blogService = require('./blog-service');

module.exports = {
    userService,
    blogService
};
```

### 4. Main Application

```javascript
// app.js
const { userService, blogService } = require('./services');
const { Logger } = require('./utils');

async function runBlogDemo() {
    try {
        Logger.info('🚀 Starting Blog System Demo');
        
        // Create users
        const alice = userService.createUser('Alice Johnson', 'alice@example.com', 'password123');
        const bob = userService.createUser('Bob Smith', 'bob@example.com', 'securepass');
        
        // Create posts
        const post1 = blogService.createPost(
            'Getting Started with Node.js',
            'Node.js is a powerful JavaScript runtime...',
            alice.id
        );
        
        const post2 = blogService.createPost(
            'Understanding Modules in Node.js',
            'Modules are the building blocks of Node.js applications...',
            bob.id
        );
        
        const post3 = blogService.createPost(
            'Advanced Node.js Patterns',
            'Let\'s explore some advanced patterns...',
            alice.id
        );
        
        // View posts
        Logger.info('\n📝 Viewing posts...');
        const viewedPost = blogService.getPost(post1.id);
        Logger.info(`Viewed: "${viewedPost.title}" - Views: ${viewedPost.views}`);
        
        // Get all posts with author info
        Logger.info('\n📚 All blog posts:');
        const allPosts = blogService.getAllPosts();
        allPosts.forEach(post => {
            console.log(`📄 "${post.title}" by ${post.author.name} (${post.views} views)`);
        });
        
        // Get posts by specific author
        Logger.info(`\n✍️  Alice's posts:`);
        const alicePosts = blogService.getPostsByAuthor(alice.id);
        alicePosts.forEach(post => {
            console.log(`📄 "${post.title}" (${post.views} views)`);
        });
        
        // Display user stats
        Logger.info('\n👥 User statistics:');
        const users = userService.getAllUsers();
        users.forEach(user => {
            console.log(`👤 ${user.name} - ${user.postCount} posts`);
        });
        
        Logger.success('\n🎉 Blog demo completed successfully!');
        
    } catch (error) {
        Logger.error(`Demo failed: ${error.message}`);
    }
}

// Run the demo
runBlogDemo();
```

**Running the Demo:**
```bash
node app.js
```

**Expected Output:**
```
ℹ️  [INFO] 2024-01-15T10:30:00.000Z: 🚀 Starting Blog System Demo
ℹ️  [INFO] 2024-01-15T10:30:00.001Z: Attempting to create user: alice@example.com
✅ [SUCCESS] 2024-01-15T10:30:00.002Z: User created: alice@example.com
ℹ️  [INFO] 2024-01-15T10:30:00.003Z: Attempting to create user: bob@example.com
✅ [SUCCESS] 2024-01-15T10:30:00.004Z: User created: bob@example.com
ℹ️  [INFO] 2024-01-15T10:30:00.005Z: Creating post: Getting Started with Node.js
✅ [SUCCESS] 2024-01-15T10:30:00.006Z: Post created: Getting Started with Node.js
...
📄 "Getting Started with Node.js" by Alice Johnson (1 views)
📄 "Understanding Modules in Node.js" by Bob Smith (0 views)
📄 "Advanced Node.js Patterns" by Alice Johnson (0 views)
...
✅ [SUCCESS] 2024-01-15T10:30:00.020Z: 🎉 Blog demo completed successfully!
```

## Module Caching Deep Dive 🗄️

Understanding how Node.js caches modules is crucial:

```javascript
// counter.js
console.log('🏗️  Counter module is being initialized!');

let count = 0;

function increment() {
    count++;
    console.log(`📈 Count is now: ${count}`);
    return count;
}

function reset() {
    count = 0;
    console.log('🔄 Counter reset!');
}

function getCount() {
    return count;
}

module.exports = {
    increment,
    reset,
    getCount
};
```

```javascript
// app.js
console.log('🚀 Starting application...\n');

console.log('📥 First require:');
const counter1 = require('./counter');

console.log('\n📥 Second require:');
const counter2 = require('./counter');

console.log('\n🧪 Testing shared state:');
counter1.increment(); // Count: 1
counter2.increment(); // Count: 2 (same instance!)
console.log('Counter1 value:', counter1.getCount()); // 2
console.log('Counter2 value:', counter2.getCount()); // 2

console.log('\n🔍 Are they the same object?', counter1 === counter2); // true
```

**Output:**
```
🚀 Starting application...

📥 First require:
🏗️  Counter module is being initialized!

📥 Second require:

🧪 Testing shared state:
📈 Count is now: 1
📈 Count is now: 2
Counter1 value: 2
Counter2 value: 2

🔍 Are they the same object? true
```

### Breaking the Cache (Advanced)

```javascript
// fresh-require.js
function requireFresh(modulePath) {
    delete require.cache[require.resolve(modulePath)];
    return require(modulePath);
}

module.exports = requireFresh;
```

```javascript
// cache-demo.js
const requireFresh = require('./fresh-require');

console.log('🔄 First fresh require:');
const counter1 = requireFresh('./counter');
counter1.increment();

console.log('\n🔄 Second fresh require:');
const counter2 = requireFresh('./counter');
counter2.increment(); // Starts from 0 again!

console.log('Are they the same?', counter1 === counter2); // false
```

## Common Patterns and Best Practices 💡

### 1. Configuration Module Pattern

```javascript
// config/database.js
const environments = {
    development: {
        host: 'localhost',
        port: 5432,
        database: 'myapp_dev',
        username: 'dev_user',
        password: 'dev_pass'
    },
    production: {
        host: process.env.DB_HOST,
        port: process.env.DB_PORT,
        database: process.env.DB_NAME,
        username: process.env.DB_USER,
        password: process.env.DB_PASS
    },
    test: {
        host: 'localhost',
        port: 5433,
        database: 'myapp_test',
        username: 'test_user',
        password: 'test_pass'
    }
};

const environment = process.env.NODE_ENV || 'development';

module.exports = environments[environment];
```

### 2. Plugin Architecture Pattern

```javascript
// plugin-manager.js
class PluginManager {
    constructor() {
        this.plugins = new Map();
    }
    
    register(name, plugin) {
        if (typeof plugin.init !== 'function') {
            throw new Error('Plugin must have an init method');
        }
        
        this.plugins.set(name, plugin);
        console.log(`🔌 Plugin registered: ${name}`);
    }
    
    initialize() {
        for (const [name, plugin] of this.plugins) {
            try {
                plugin.init();
                console.log(`✅ Plugin initialized: ${name}`);
            } catch (error) {
                console.error(`❌ Failed to initialize plugin ${name}:`, error.message);
            }
        }
    }
    
    get(name) {
        return this.plugins.get(name);
    }
}

module.exports = new PluginManager();
```

```javascript
// plugins/email-plugin.js
module.exports = {
    name: 'Email Plugin',
    
    init() {
        this.templates = new Map();
        console.log('📧 Email plugin ready!');
    },
    
    addTemplate(name, template) {
        this.templates.set(name, template);
    },
    
    send(to, template, data = {}) {
        const emailTemplate = this.templates.get(template);
        if (!emailTemplate) {
            throw new Error(`Template ${template} not found`);
        }
        
        console.log(`📤 Sending ${template} email to ${to}`);
        return { status: 'sent', to, template };
    }
};
```

```javascript
// plugins/logging-plugin.js
module.exports = {
    name: 'Logging Plugin',
    
    init() {
        this.logs = [];
        console.log('📝 Logging plugin ready!');
    },
    
    log(level, message, metadata = {}) {
        const logEntry = {
            timestamp: new Date(),
            level,
            message,
            metadata
        };
        
        this.logs.push(logEntry);
        console.log(`[${level}] ${message}`);
    },
    
    getLogs(level = null) {
        if (level) {
            return this.logs.filter(log => log.level === level);
        }
        return this.logs;
    }
};
```

```javascript
// app-with-plugins.js
const pluginManager = require('./plugin-manager');
const emailPlugin = require('./plugins/email-plugin');
const loggingPlugin = require('./plugins/logging-plugin');

// Register plugins
pluginManager.register('email', emailPlugin);
pluginManager.register('logging', loggingPlugin);

// Initialize all plugins
pluginManager.initialize();

// Use plugins
const email = pluginManager.get('email');
const logger = pluginManager.get('logging');

email.addTemplate('welcome', 'Welcome to our service!');
logger.log('INFO', 'Application started');

email.send('user@example.com', 'welcome');
logger.log('INFO', 'Welcome email sent');
```

## Troubleshooting Common Issues 🛠️

### 1. Circular Dependencies

**❌ Problem:**
```javascript
// a.js
const b = require('./b');
console.log('A:', b.name);
module.exports = { name: 'Module A' };

// b.js
const a = require('./a');
console.log('B:', a.name); // undefined!
module.exports = { name: 'Module B' };
```

**✅ Solution:**
```javascript
// a.js
console.log('A loading...');
module.exports = { name: 'Module A' };
const b = require('./b'); // Require after export

// b.js  
console.log('B loading...');
module.exports = { name: 'Module B' };
const a = require('./a'); // Require after export
```

### 2. Path Resolution Issues

**❌ Problem:**
```javascript
// Wrong - missing './'
const helper = require('helper'); // Looks in node_modules!
```

**✅ Solution:**
```javascript
// Correct - relative path
const helper = require('./helper');
const config = require('../config');
const utils = require('./utils/index');
```

### 3. Module Not Found Errors

**Debugging require.resolve():**
```javascript
// debug-require.js
function debugRequire(moduleName) {
    try {
        const resolved = require.resolve(moduleName);
        console.log(`✅ Module found: ${resolved}`);
        return require(moduleName);
    } catch (error) {
        console.error(`❌ Module not found: ${moduleName}`);
        console.error('Search paths:', module.paths);
        throw error;
    }
}

// Usage
const myModule = debugRequire('./my-module');
const express = debugRequire('express');
```

## Performance Optimization Tips ⚡

### 1. Lazy Loading

```javascript
// heavy-processor.js
console.log('🏗️  Heavy processor module loaded!');

function processLargeData(data) {
    // Simulate heavy processing
    console.log('🔄 Processing large data...');
    return data.map(x => x * x).filter(x => x > 100);
}

module.exports = { processLargeData };
```

```javascript
// app-with-lazy-loading.js
let heavyProcessor = null;

function processDataWhenNeeded(data) {
    // Only load the heavy module when actually needed
    if (!heavyProcessor) {
        console.log('📦 Loading heavy processor...');
        heavyProcessor = require('./heavy-processor');
    }
    
    return heavyProcessor.processLargeData(data);
}

console.log('🚀 App started (heavy processor not loaded yet)');

// Heavy module only loads when this function is called
setTimeout(() => {
    const result = processDataWhenNeeded([1, 5, 15, 25]);
    console.log('Result:', result);
}, 2000);
```

### 2. Module Bundling Strategy

```javascript
// utils/index.js - Barrel exports
// Only export what's commonly used together
module.exports = {
    // Commonly used utilities
    ...require('./string-utils'),
    ...require('./date-utils'),
    
    // Heavy utilities - lazy load
    get imageProcessor() {
        return require('./image-processor');
    },
    
    get pdfGenerator() {
        return require('./pdf-generator');
    }
};
```

---

## 🎉 Mastery Achieved!

Congratulations! You've mastered the art of `require` and `export` in Node.js! You now understand:

- 📤 **How to export** functions, objects, classes, and values
- 📥 **How to require** modules using different patterns
- 🏗️ **Module architecture** for real-world applications
- 🔄 **Module caching** and how it affects your code
- 🛠️ **Troubleshooting** common require/export issues
- ⚡ **Performance optimization** techniques

### Key Takeaways:
- **`module.exports`** is your way of sharing code between files
- **`require()`** is your way of using code from other files
- **Relative paths** (`./`, `../`) for your own modules
- **Module names** for built-in and npm modules
- **Modules are cached** - they only execute once
- **Good organization** makes large projects manageable

### What's Next?
- Explore **ES6 modules** (`import`/`export`)
- Learn about **webpack** and module bundling
- Build larger applications using these patterns
- Contribute to open-source npm packages

You're now ready to build modular, maintainable Node.js applications! Start creating your own modules and share them with the world.

**Happy Module Building!** 🚀✨
