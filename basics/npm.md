# Node Package Manager (npm) 📦

Welcome to the complete guide on npm - the package manager that powers the Node.js ecosystem! This guide will transform you from an npm beginner to a confident package manager pro.

## What is npm? 🤔

npm (Node Package Manager) is like the **App Store for JavaScript developers**! Just as you download apps on your phone, npm lets you download and manage JavaScript packages (modules) for your projects.

Think of npm as your **digital toolbox** where you can:
- 🔍 **Find tools** (packages) created by developers worldwide
- 📦 **Download and install** them in your projects
- 🔄 **Update** them when new versions are released
- 🗑️ **Remove** them when no longer needed
- 📋 **Manage dependencies** automatically

> **Fun Fact:** npm is the world's largest software registry with over 2 million packages! It's like having access to millions of pre-built solutions.

![npm Registry](https://via.placeholder.com/600x300/CB3837/FFFFFF?text=npm+Registry+-+2M%2B+Packages+%F0%9F%93%A6)

## npm Components 🧩

### 1. The Registry 🗂️
A massive online database of JavaScript packages hosted at [npmjs.com](https://npmjs.com)

### 2. The CLI (Command Line Interface) 💻
The tool you use in your terminal to interact with npm

### 3. package.json 📋
Your project's configuration file that tracks dependencies and metadata

## Getting Started with npm 🚀

npm comes bundled with Node.js, so if you have Node.js installed, you already have npm!

### Check Your npm Version

```bash
# Check npm version
npm --version
# or
npm -v

# Expected output: 9.6.7 (or similar)
```

### Update npm

```bash
# Update npm to latest version
npm install -g npm@latest

# Check available npm versions
npm view npm versions --json
```

## Understanding package.json 📄

The `package.json` file is the **heart** of your Node.js project. It's like an ID card for your project!

### Creating package.json

```bash
# Interactive creation (recommended for beginners)
npm init

# Quick creation with defaults
npm init -y
# or
npm init --yes
```

**Interactive npm init example:**
```bash
$ npm init
package name: (my-awesome-app) 
version: (1.0.0) 
description: My first Node.js application
entry point: (index.js) 
test command: npm test
git repository: https://github.com/username/my-awesome-app
keywords: nodejs, beginner, awesome
author: Your Name <your.email@example.com>
license: (ISC) MIT
```

### Sample package.json

```json
{
  "name": "my-awesome-app",
  "version": "1.0.0",
  "description": "My first Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest",
    "build": "webpack --mode production"
  },
  "keywords": ["nodejs", "beginner", "awesome"],
  "author": "Your Name <your.email@example.com>",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "nodemon": "^2.0.22",
    "jest": "^29.5.0"
  }
}
```

## Installing Packages 📥

### Local Installation (Project-specific)

```bash
# Install a package for your project
npm install express
# or shorter version
npm i express

# Install multiple packages at once
npm install express mongoose cors helmet

# Install and save as dependency (default behavior)
npm install --save lodash
npm install -S lodash

# Install as development dependency
npm install --save-dev nodemon
npm install -D nodemon
```

**What happens when you install locally?**
- Package is downloaded to `node_modules/` folder
- Added to `dependencies` or `devDependencies` in package.json
- Available only in the current project

### Global Installation (System-wide)

```bash
# Install globally (available everywhere)
npm install -g nodemon
npm install --global create-react-app
npm i -g typescript

# List global packages
npm list -g --depth=0
```

**When to install globally?**
- ✅ Command-line tools (nodemon, create-react-app, typescript)
- ✅ Development utilities you use across projects
- ❌ Project dependencies (express, lodash, etc.)

### Installing from package.json

```bash
# Install all dependencies listed in package.json
npm install
# or simply
npm i

# Install only production dependencies
npm install --production
npm install --only=prod
```

## Essential npm Commands 🛠️

### Package Management

```bash
# Install specific version
npm install lodash@4.17.20

# Install version range
npm install express@^4.18.0  # Compatible with 4.18.0
npm install lodash@~4.17.20  # Compatible with 4.17.x

# Install from GitHub
npm install user/repo
npm install https://github.com/user/repo.git

# Install from local folder
npm install ../my-local-package
npm install ./packages/my-package
```

### Updating Packages

```bash
# Check for outdated packages
npm outdated

# Update all packages
npm update

# Update specific package
npm update express

# Update to latest version (ignoring semver)
npm install express@latest
```

### Removing Packages

```bash
# Remove package
npm uninstall express
npm remove express
npm rm express

# Remove and update package.json
npm uninstall --save express
npm uninstall -S express

# Remove dev dependency
npm uninstall --save-dev nodemon
npm uninstall -D nodemon

# Remove global package
npm uninstall -g create-react-app
```

## Package Information Commands 🔍

```bash
# View package information
npm view express
npm info lodash

# View package versions
npm view express versions
npm view react versions --json

# Show package documentation
npm docs express
npm home lodash

# Search for packages
npm search "web framework"
npm search express

# List installed packages
npm list
npm ls

# List with specific depth
npm ls --depth=0

# List global packages
npm ls -g --depth=0
```

## Understanding Semantic Versioning (SemVer) 🏷️

npm uses semantic versioning: **MAJOR.MINOR.PATCH**

```
Version: 4.18.2
         │ │  │
         │ │  └─ PATCH: Bug fixes
         │ └─── MINOR: New features (backward compatible)  
         └───── MAJOR: Breaking changes
```

### Version Range Symbols

```json
{
  "dependencies": {
    "express": "4.18.2",      // Exact version
    "lodash": "^4.17.21",     // Compatible with 4.17.21 (^4.17.21 ≤ version < 5.0.0)
    "moment": "~2.29.4",      // Approximately 2.29.4 (~2.29.4 ≤ version < 2.30.0)
    "axios": ">=1.0.0",       // Greater than or equal to 1.0.0
    "react": "*"              // Latest version (not recommended)
  }
}
```

**Best Practices:**
- ✅ Use `^` for minor updates (default)
- ✅ Use `~` for patch updates only
- ✅ Use exact versions for critical dependencies
- ❌ Avoid `*` in production

## npm Scripts 📜

npm scripts are shortcuts for running commands in your project.

### Common Scripts

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon app.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "build": "webpack --mode production",
    "build:dev": "webpack --mode development",
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "clean": "rm -rf dist/",
    "deploy": "npm run build && npm run test"
  }
}
```

### Running Scripts

```bash
# Run npm scripts
npm run start
npm run dev
npm run build
npm run test

# Special scripts (can omit 'run')
npm start    # Same as npm run start
npm test     # Same as npm run test
npm restart  # Runs stop, start
npm stop     # Runs prestop, stop, poststop
```

### Script Hooks

```json
{
  "scripts": {
    "prebuild": "npm run clean",     // Runs before build
    "build": "webpack",              // Main build script
    "postbuild": "npm run test",     // Runs after build
    
    "pretest": "npm run lint",       // Runs before test
    "test": "jest",                  // Main test script
    "posttest": "echo 'Tests completed!'"  // Runs after test
  }
}
```

### Passing Arguments to Scripts

```bash
# Pass arguments to scripts
npm run test -- --watch
npm run build -- --mode=development

# In package.json
{
  "scripts": {
    "serve": "http-server -p"
  }
}

# Usage
npm run serve 8080  # Serves on port 8080
```

## Advanced npm Features 🚀

### npm Configuration

```bash
# View npm configuration
npm config list
npm config list -l  # Show all configs

# Set configuration
npm config set registry https://registry.npmjs.org/
npm config set init-author-name "Your Name"
npm config set init-author-email "your.email@example.com"
npm config set init-license "MIT"

# Get specific config
npm config get registry

# Delete config
npm config delete proxy
```

### Working with .npmrc

Create a `.npmrc` file in your project or home directory:

```bash
# .npmrc file
registry=https://registry.npmjs.org/
save-exact=true
init-author-name=Your Name
init-author-email=your.email@example.com
init-license=MIT
```

### npm Cache Management

```bash
# View cache info
npm cache verify

# Clean cache
npm cache clean --force

# View cache folder
npm config get cache
```

### Package-lock.json 🔒

The `package-lock.json` file ensures **exact** dependency versions across different environments.

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "lockfileVersion": 2,
  "requires": true,
  "packages": {
    "": {
      "name": "my-app",
      "version": "1.0.0",
      "dependencies": {
        "express": "^4.18.2"
      }
    },
    "node_modules/express": {
      "version": "4.18.2",
      "resolved": "https://registry.npmjs.org/express/-/express-4.18.2.tgz",
      "integrity": "sha512-5/PsL6iGPdfQ/lKM1UuielYgv3BUoJfz1aUwU9vHZ+J7gyvwdQXFEBIEIaxeGf0GIcreATNyBExtalisDbuMqQ=="
    }
  }
}
```

**Key Points:**
- ✅ **Always commit** package-lock.json to version control
- ✅ Use `npm ci` for production deployments
- ❌ Don't manually edit package-lock.json

### Production Deployments

```bash
# Clean install (faster, uses package-lock.json exactly)
npm ci

# Install only production dependencies
npm ci --production
npm ci --only=production
```

## Creating Your First Package 📦

Let's create and publish a simple utility package!

### 1. Initialize Your Package

```bash
mkdir my-awesome-utils
cd my-awesome-utils
npm init
```

### 2. Create Your Module

```javascript
// index.js
/**
 * My Awesome Utils - A collection of helpful utilities
 */

/**
 * Generate a random number between min and max
 * @param {number} min - Minimum value
 * @param {number} max - Maximum value  
 * @returns {number} Random number
 */
function randomBetween(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

/**
 * Capitalize the first letter of a string
 * @param {string} str - Input string
 * @returns {string} Capitalized string
 */
function capitalize(str) {
    if (!str || typeof str !== 'string') return str;
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

/**
 * Check if a string is a valid email
 * @param {string} email - Email to validate
 * @returns {boolean} True if valid email
 */
function isValidEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
}

/**
 * Delay execution for specified milliseconds
 * @param {number} ms - Milliseconds to wait
 * @returns {Promise} Promise that resolves after delay
 */
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

module.exports = {
    randomBetween,
    capitalize,
    isValidEmail,
    delay
};
```

### 3. Update package.json

```json
{
  "name": "my-awesome-utils",
  "version": "1.0.0",
  "description": "A collection of helpful utility functions",
  "main": "index.js",
  "scripts": {
    "test": "node test.js"
  },
  "keywords": ["utils", "utilities", "helpers", "tools"],
  "author": "Your Name <your.email@example.com>",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/my-awesome-utils.git"
  },
  "bugs": {
    "url": "https://github.com/yourusername/my-awesome-utils/issues"
  },
  "homepage": "https://github.com/yourusername/my-awesome-utils#readme"
}
```

### 4. Create Tests

```javascript
// test.js
const { randomBetween, capitalize, isValidEmail, delay } = require('./index');

console.log('🧪 Testing my-awesome-utils...\n');

// Test randomBetween
console.log('Testing randomBetween(1, 10):', randomBetween(1, 10));
console.log('Testing randomBetween(100, 200):', randomBetween(100, 200));

// Test capitalize
console.log('Testing capitalize("hello world"):', capitalize("hello world"));
console.log('Testing capitalize("JAVASCRIPT"):', capitalize("JAVASCRIPT"));

// Test isValidEmail
console.log('Testing isValidEmail("test@example.com"):', isValidEmail("test@example.com"));
console.log('Testing isValidEmail("invalid-email"):', isValidEmail("invalid-email"));

// Test delay
console.log('Testing delay(1000ms)...');
delay(1000).then(() => {
    console.log('✅ Delay completed!');
    console.log('\n🎉 All tests passed!');
});
```

### 5. Create README.md

```markdown
# My Awesome Utils

A collection of helpful utility functions for JavaScript projects.

## Installation

```bash
npm install my-awesome-utils
```

## Usage

```javascript
const { randomBetween, capitalize, isValidEmail, delay } = require('my-awesome-utils');

// Generate random number
console.log(randomBetween(1, 100)); // Random number between 1-100

// Capitalize string
console.log(capitalize('hello world')); // "Hello world"

// Validate email
console.log(isValidEmail('test@example.com')); // true

// Delay execution
delay(1000).then(() => {
    console.log('This runs after 1 second');
});
```

## API

### randomBetween(min, max)
Generates a random integer between min and max (inclusive).

### capitalize(str)
Capitalizes the first letter of a string.

### isValidEmail(email)
Validates if a string is a valid email format.

### delay(ms)
Returns a promise that resolves after the specified milliseconds.
```

## Publishing Packages 🚀

### 1. Create npm Account

```bash
# Create account on npmjs.com first, then:
npm login

# Check if logged in
npm whoami
```

### 2. Publish Your Package

```bash
# Make sure everything is ready
npm run test

# Publish to npm registry
npm publish

# Success message:
# + my-awesome-utils@1.0.0
```

### 3. Update Your Package

```bash
# Update version (patch: 1.0.0 -> 1.0.1)
npm version patch

# Update version (minor: 1.0.1 -> 1.1.0) 
npm version minor

# Update version (major: 1.1.0 -> 2.0.0)
npm version major

# Publish updated version
npm publish
```

### 4. Unpublish (Use Carefully!)

```bash
# Unpublish specific version (within 72 hours)
npm unpublish my-awesome-utils@1.0.0

# Unpublish entire package (within 72 hours)
npm unpublish my-awesome-utils --force
```

## Working with Different Registries 🌐

### Using Different Registries

```bash
# Use different registry for single install
npm install --registry https://registry.npmjs.org/ lodash

# Set registry globally
npm config set registry https://registry.npmjs.org/

# Use yarn registry
npm config set registry https://registry.yarnpkg.com/

# Reset to default
npm config set registry https://registry.npmjs.org/
```

### Private Registries

```bash
# Login to private registry
npm login --registry https://my-private-registry.com/

# Install from private registry
npm install @mycompany/private-package --registry https://my-private-registry.com/

# Publish to private registry
npm publish --registry https://my-private-registry.com/
```

## npm Workspaces (Monorepos) 🏢

Manage multiple packages in a single repository:

```json
{
  "name": "my-monorepo",
  "workspaces": [
    "packages/*",
    "apps/*"
  ]
}
```

**Directory structure:**
```
my-monorepo/
├── package.json
├── packages/
│   ├── utils/
│   │   └── package.json
│   └── components/
│       └── package.json
└── apps/
    ├── web/
    │   └── package.json
    └── mobile/
        └── package.json
```

```bash
# Install dependencies for all workspaces
npm install

# Run script in specific workspace
npm run build --workspace=packages/utils

# Add dependency to specific workspace
npm install lodash --workspace=packages/utils
```

## Troubleshooting Common Issues 🛠️

### 1. Permission Errors (EACCES)

```bash
# Fix npm permissions on macOS/Linux
sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}

# Or use a Node version manager (recommended)
# Install nvm and reinstall Node.js
```

### 2. Package Not Found

```bash
# Check spelling
npm search express

# Clear cache
npm cache clean --force

# Check registry
npm config get registry
```

### 3. Version Conflicts

```bash
# Check for duplicates
npm ls

# Dedupe packages
npm dedupe

# Fresh install
rm -rf node_modules package-lock.json
npm install
```

### 4. Slow Installation

```bash
# Use different registry
npm install --registry https://registry.npmjs.org/

# Clear cache
npm cache clean --force

# Check internet connection
npm ping
```

### 5. Peer Dependency Warnings

```bash
# Install peer dependencies manually
npm install react react-dom  # If package requires these

# Or use --legacy-peer-deps flag
npm install --legacy-peer-deps
```

## npm Best Practices 💡

### 1. Security

```bash
# Audit packages for vulnerabilities
npm audit

# Fix vulnerabilities automatically
npm audit fix

# Install specific vulnerability fixes
npm audit fix --force

# Check for known security issues
npm install -g npm-check-updates
ncu --doctor
```

### 2. Performance

```bash
# Use npm ci in production
npm ci --production

# Enable package-lock.json
npm config set package-lock true

# Use .npmignore for publishing
echo "tests/" >> .npmignore
echo "*.test.js" >> .npmignore
```

### 3. Development Workflow

```json
{
  "scripts": {
    "precommit": "npm run lint && npm test",
    "lint": "eslint .",
    "test": "jest",
    "dev": "nodemon app.js",
    "start": "node app.js",
    "build": "webpack --mode production"
  }
}
```

## Useful npm Tools and Packages 🧰

### Development Tools

```bash
# Package management
npm install -g npm-check-updates  # Check for updates
npm install -g npm-check         # Interactive package checker

# Development servers
npm install -g live-server       # Live reload server
npm install -g http-server      # Simple HTTP server

# Build tools
npm install -g nodemon          # Auto-restart on changes
npm install -g concurrently     # Run multiple commands
```

### Useful Packages for Every Project

```bash
# Utilities
npm install lodash              # Utility library
npm install moment             # Date manipulation
npm install axios              # HTTP client

# Development
npm install -D nodemon         # Development server
npm install -D jest            # Testing framework
npm install -D eslint          # Code linting
```

## Quick Reference Commands 📋

### Installation Commands
```bash
npm install package-name        # Install package
npm install -g package-name     # Install globally
npm install -D package-name     # Install as dev dependency
npm install package@version     # Install specific version
npm ci                         # Clean install from lock file
```

### Information Commands
```bash
npm list                       # List installed packages
npm outdated                   # Check for updates
npm audit                      # Security audit
npm view package-name          # Package information
npm search keyword             # Search packages
```

### Management Commands
```bash
npm update                     # Update packages
npm uninstall package-name     # Remove package
npm run script-name           # Run npm script
npm version patch/minor/major # Update version
npm publish                   # Publish package
```

### Configuration Commands
```bash
npm config list               # Show configuration
npm config set key value      # Set configuration
npm config get key           # Get configuration value
npm login                    # Login to registry
npm whoami                   # Check logged in user
```

---

## 🎉 You're Now an npm Expert!

Congratulations! You've mastered npm, the powerful package manager that drives the Node.js ecosystem. With these skills, you can:

- 📦 **Manage packages** efficiently in your projects
- 🔧 **Use npm scripts** to automate your workflow  
- 🚀 **Create and publish** your own packages
- 🛠️ **Troubleshoot** common npm issues
- 💡 **Follow best practices** for security and performance

### Key Takeaways:
- npm is your gateway to millions of JavaScript packages
- `package.json` is the heart of your Node.js project
- Always commit `package-lock.json` for consistent installations
- Use semantic versioning to manage updates safely
- npm scripts automate repetitive tasks
- Security audits keep your projects safe

Start exploring the npm ecosystem and build amazing things with the power of open-source packages!

**Happy Package Managing!** 📦✨
