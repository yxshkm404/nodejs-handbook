# Node.js Server Development 🚀

Welcome to the exciting world of server development with Node.js! Think of this as your journey from zero to building powerful web servers that can handle real-world applications.

## What is a Server? 🤔

Imagine a **restaurant** 🍽️. When customers (clients) come in and place orders (requests), the kitchen staff (server) processes these orders and serves the food back (responses). That's exactly how a web server works!

A server is a **computer program** that:
- 👂 **Listens** for incoming requests
- 🧠 **Processes** those requests  
- 📤 **Sends back** appropriate responses
- 🔄 **Repeats** this cycle continuously

![Server Restaurant Analogy](https://mobifly.tech/wp-content/uploads/2021/05/api-restaurant-analogy-example1-1024x640.jpg)

> **Real-world analogy:** Your favorite food delivery app! When you open the app (client) and search for restaurants (request), the app's servers process your location, find nearby restaurants, and send back the results (response). When you place an order, that's another request-response cycle!

## How Web Communication Works 🌐
![web communication working](https://substackcdn.com/image/fetch/$s_!g3db!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a38175b-11e8-40ae-879c-ab3ce2027089_2008x1252.png)

### The Request-Response Cycle

Every web interaction follows this simple pattern:

1. **🔍 Client makes a request** - "Hey server, I want the homepage!"
2. **⚡ Server processes the request** - "Let me get that for you..."
3. **📦 Server sends a response** - "Here's your homepage!"
4. **🎉 Client receives and displays** - User sees the webpage

```javascript
// Simple explanation in code
const conversation = {
    client: "GET /homepage HTTP/1.1",
    server: "HTTP/1.1 200 OK\nContent: <html>Welcome!</html>"
};
```

### What Happens Behind the Scenes?

```javascript
// When you visit www.example.com in your browser:

// 1. Browser (Client) creates a request
const request = {
    method: 'GET',
    url: 'https://www.example.com/',
    headers: {
        'User-Agent': 'Mozilla/5.0...',
        'Accept': 'text/html,application/json'
    }
};

// 2. Server processes and creates response
const response = {
    statusCode: 200,
    headers: {
        'Content-Type': 'text/html',
        'Content-Length': '1234'
    },
    body: '<html><body><h1>Hello World!</h1></body></html>'
};

// 3. Browser renders the HTML to show the webpage
```

## Your First Node.js Server 🎯

Let's start with the simplest possible server:

### Hello World Server

```javascript
// hello-server.js
const http = require('http');

// Create a server
const server = http.createServer((request, response) => {
    console.log('🎉 Someone visited our server!');
    
    // Set response headers
    response.writeHead(200, { 'Content-Type': 'text/html' });
    
    // Send response
    response.end('<h1>Hello World! 🌍</h1><p>Your first Node.js server is working!</p>');
});

// Start listening on port 3000
const PORT = 3000;
server.listen(PORT, () => {
    console.log(`🚀 Server is running at http://localhost:${PORT}`);
    console.log('💡 Press Ctrl+C to stop the server');
});
```

**Run your server:**
```bash
node hello-server.js
```

**Open your browser and visit:** `http://localhost:3000`

**🎉 Congratulations!** You just built your first web server!

### Understanding the Code

```javascript
// Let's break down what each part does:

const http = require('http');
// 📦 Import Node.js built-in HTTP module

const server = http.createServer((request, response) => {
    // 🏗️ Create server with callback function
    // This function runs EVERY TIME someone visits your server
    
    response.writeHead(200, { 'Content-Type': 'text/html' });
    // 📝 Set status code (200 = success) and content type
    
    response.end('Hello World!');
    // 📤 Send response and close connection
});

server.listen(3000, callback);
// 👂 Start listening on port 3000
```

## Understanding HTTP Methods 📡

HTTP methods are like **different types of restaurant orders**:

### GET - "I want to see the menu" 👀

```javascript
// get-server.js
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.method === 'GET' && req.url === '/') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <head><title>My Restaurant</title></head>
                <body>
                    <h1>🍕 Welcome to Tony's Pizza!</h1>
                    <h2>📋 Menu:</h2>
                    <ul>
                        <li>🍕 Margherita Pizza - $12</li>
                        <li>🍝 Spaghetti Carbonara - $14</li>
                        <li>🥗 Caesar Salad - $8</li>
                        <li>🍰 Tiramisu - $6</li>
                    </ul>
                    <p>Visit <a href="/about">/about</a> to learn more!</p>
                </body>
            </html>
        `);
    } else if (req.method === 'GET' && req.url === '/about') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <body>
                    <h1>About Tony's Pizza</h1>
                    <p>🇮🇹 Authentic Italian cuisine since 1985!</p>
                    <p><a href="/">← Back to Menu</a></p>
                </body>
            </html>
        `);
    } else {
        res.writeHead(404, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <body>
                    <h1>404 - Page Not Found 😞</h1>
                    <p>Oops! This page doesn't exist.</p>
                    <p><a href="/">← Go Home</a></p>
                </body>
            </html>
        `);
    }
});

server.listen(3000, () => {
    console.log('🍕 Tony\'s Pizza server is running at http://localhost:3000');
});
```

### POST - "I want to place an order" 📝

```javascript
// post-server.js
const http = require('http');
const url = require('url');
const querystring = require('querystring');

// In-memory storage for orders (in real apps, use databases!)
let orders = [];
let orderIdCounter = 1;

const server = http.createServer((req, res) => {
    const parsedUrl = url.parse(req.url);
    const path = parsedUrl.pathname;
    
    if (req.method === 'GET' && path === '/') {
        // Show order form
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <head><title>Pizza Order Form</title></head>
                <body>
                    <h1>🍕 Order Your Pizza!</h1>
                    <form method="POST" action="/order">
                        <p>
                            <label>Your Name:</label><br>
                            <input type="text" name="name" required>
                        </p>
                        <p>
                            <label>Pizza Type:</label><br>
                            <select name="pizza" required>
                                <option value="">Choose...</option>
                                <option value="margherita">🍕 Margherita</option>
                                <option value="pepperoni">🍕 Pepperoni</option>
                                <option value="veggie">🥬 Veggie Supreme</option>
                            </select>
                        </p>
                        <p>
                            <label>Size:</label><br>
                            <input type="radio" name="size" value="small" required> Small
                            <input type="radio" name="size" value="medium" required> Medium
                            <input type="radio" name="size" value="large" required> Large
                        </p>
                        <p>
                            <button type="submit">🛒 Place Order</button>
                        </p>
                    </form>
                    
                    <hr>
                    <h2>📋 Current Orders:</h2>
                    ${orders.length === 0 ? '<p>No orders yet!</p>' : 
                      orders.map(order => `
                        <div style="border: 1px solid #ccc; padding: 10px; margin: 5px;">
                            <strong>Order #${order.id}</strong><br>
                            Customer: ${order.name}<br>
                            Pizza: ${order.pizza} (${order.size})<br>
                            Time: ${order.timestamp}
                        </div>
                      `).join('')
                    }
                </body>
            </html>
        `);
        
    } else if (req.method === 'POST' && path === '/order') {
        // Handle form submission
        let body = '';
        
        req.on('data', chunk => {
            body += chunk.toString();
        });
        
        req.on('end', () => {
            const formData = querystring.parse(body);
            
            // Create new order
            const order = {
                id: orderIdCounter++,
                name: formData.name,
                pizza: formData.pizza,
                size: formData.size,
                timestamp: new Date().toLocaleString()
            };
            
            orders.push(order);
            
            console.log('🎉 New order received:', order);
            
            // Redirect back to form with success message
            res.writeHead(302, { 'Location': '/' });
            res.end();
        });
        
    } else {
        // 404 for unknown routes
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Page not found');
    }
});

server.listen(3000, () => {
    console.log('🍕 Pizza order server running at http://localhost:3000');
});
```

### PUT - "I want to update my order" 🔄

```javascript
// put-server.js
const http = require('http');
const url = require('url');

let orders = [
    { id: 1, name: 'John', pizza: 'margherita', size: 'large', status: 'preparing' },
    { id: 2, name: 'Sarah', pizza: 'pepperoni', size: 'medium', status: 'baking' }
];

const server = http.createServer((req, res) => {
    const parsedUrl = url.parse(req.url);
    const path = parsedUrl.pathname;
    
    // Enable CORS for testing with tools like Postman
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
    
    if (req.method === 'GET' && path === '/') {
        // Show all orders
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <body>
                    <h1>📋 Pizza Orders Management</h1>
                    <div>
                        ${orders.map(order => `
                            <div style="border: 1px solid #ccc; padding: 10px; margin: 10px;">
                                <h3>Order #${order.id}</h3>
                                <p><strong>Customer:</strong> ${order.name}</p>
                                <p><strong>Pizza:</strong> ${order.pizza} (${order.size})</p>
                                <p><strong>Status:</strong> 
                                    <span style="color: ${order.status === 'ready' ? 'green' : 'orange'}">
                                        ${order.status}
                                    </span>
                                </p>
                                <button onclick="updateStatus(${order.id})">Update Status</button>
                            </div>
                        `).join('')}
                    </div>
                    
                    <script>
                        function updateStatus(orderId) {
                            const newStatus = prompt('Enter new status (preparing/baking/ready):');
                            if (newStatus) {
                                fetch('/orders/' + orderId, {
                                    method: 'PUT',
                                    headers: { 'Content-Type': 'application/json' },
                                    body: JSON.stringify({ status: newStatus })
                                }).then(() => location.reload());
                            }
                        }
                    </script>
                </body>
            </html>
        `);
        
    } else if (req.method === 'PUT' && path.startsWith('/orders/')) {
        // Update order
        const orderId = parseInt(path.split('/')[2]);
        let body = '';
        
        req.on('data', chunk => {
            body += chunk.toString();
        });
        
        req.on('end', () => {
            const updateData = JSON.parse(body);
            const orderIndex = orders.findIndex(order => order.id === orderId);
            
            if (orderIndex !== -1) {
                orders[orderIndex] = { ...orders[orderIndex], ...updateData };
                console.log(`📝 Order ${orderId} updated:`, orders[orderIndex]);
                
                res.writeHead(200, { 'Content-Type': 'application/json' });
                res.end(JSON.stringify({ success: true, order: orders[orderIndex] }));
            } else {
                res.writeHead(404, { 'Content-Type': 'application/json' });
                res.end(JSON.stringify({ error: 'Order not found' }));
            }
        });
        
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Not found');
    }
});

server.listen(3000, () => {
    console.log('📝 Order management server running at http://localhost:3000');
});
```

### DELETE - "I want to cancel my order" ❌

```javascript
// delete-server.js
const http = require('http');
const url = require('url');

let orders = [
    { id: 1, name: 'John', pizza: 'margherita', size: 'large', status: 'preparing' },
    { id: 2, name: 'Sarah', pizza: 'pepperoni', size: 'medium', status: 'baking' },
    { id: 3, name: 'Mike', pizza: 'veggie', size: 'small', status: 'ready' }
];

const server = http.createServer((req, res) => {
    const parsedUrl = url.parse(req.url);
    const path = parsedUrl.pathname;
    
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
    
    if (req.method === 'OPTIONS') {
        // Handle preflight requests
        res.writeHead(200);
        res.end();
        return;
    }
    
    if (req.method === 'GET' && path === '/') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <body>
                    <h1>🗑️ Order Cancellation System</h1>
                    <div id="orders">
                        ${orders.map(order => `
                            <div id="order-${order.id}" style="border: 1px solid #ccc; padding: 15px; margin: 10px;">
                                <h3>Order #${order.id}</h3>
                                <p><strong>Customer:</strong> ${order.name}</p>
                                <p><strong>Order:</strong> ${order.pizza} pizza (${order.size})</p>
                                <p><strong>Status:</strong> ${order.status}</p>
                                <button onclick="cancelOrder(${order.id})" 
                                        style="background: red; color: white; padding: 5px 10px;">
                                    ❌ Cancel Order
                                </button>
                            </div>
                        `).join('')}
                    </div>
                    
                    <script>
                        async function cancelOrder(orderId) {
                            if (confirm('Are you sure you want to cancel this order?')) {
                                try {
                                    const response = await fetch('/orders/' + orderId, {
                                        method: 'DELETE'
                                    });
                                    
                                    const result = await response.json();
                                    
                                    if (result.success) {
                                        document.getElementById('order-' + orderId).style.display = 'none';
                                        alert('✅ Order cancelled successfully!');
                                    } else {
                                        alert('❌ Failed to cancel order: ' + result.error);
                                    }
                                } catch (error) {
                                    alert('❌ Error: ' + error.message);
                                }
                            }
                        }
                    </script>
                </body>
            </html>
        `);
        
    } else if (req.method === 'DELETE' && path.startsWith('/orders/')) {
        const orderId = parseInt(path.split('/')[2]);
        const orderIndex = orders.findIndex(order => order.id === orderId);
        
        if (orderIndex !== -1) {
            const cancelledOrder = orders.splice(orderIndex, 1)[0];
            console.log('🗑️ Order cancelled:', cancelledOrder);
            
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ 
                success: true, 
                message: `Order #${orderId} has been cancelled`,
                cancelledOrder 
            }));
        } else {
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ 
                success: false, 
                error: 'Order not found' 
            }));
        }
        
    } else if (req.method === 'GET' && path === '/api/orders') {
        // API endpoint to get all orders as JSON
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ orders }));
        
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Page not found');
    }
});

server.listen(3000, () => {
    console.log('🗑️ Order cancellation server running at http://localhost:3000');
    console.log('📊 API endpoint: http://localhost:3000/api/orders');
});
```

## Building a Complete RESTful API 🎯

Let's combine all HTTP methods into a complete pizza restaurant API:

```javascript
// pizza-restaurant-api.js
const http = require('http');
const url = require('url');
const querystring = require('querystring');

// In-memory database (use real databases in production!)
let orders = [];
let menu = [
    { id: 1, name: 'Margherita', price: 12, ingredients: ['tomato', 'mozzarella', 'basil'] },
    { id: 2, name: 'Pepperoni', price: 15, ingredients: ['tomato', 'mozzarella', 'pepperoni'] },
    { id: 3, name: 'Veggie Supreme', price: 18, ingredients: ['tomato', 'mozzarella', 'peppers', 'onions', 'mushrooms'] }
];
let orderIdCounter = 1;

// Utility function to parse JSON from request
function parseJSONBody(req) {
    return new Promise((resolve, reject) => {
        let body = '';
        req.on('data', chunk => body += chunk.toString());
        req.on('end', () => {
            try {
                resolve(JSON.parse(body));
            } catch (error) {
                reject(error);
            }
        });
    });
}

// Utility function to send JSON response
function sendJSON(res, statusCode, data) {
    res.writeHead(statusCode, { 
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE',
        'Access-Control-Allow-Headers': 'Content-Type'
    });
    res.end(JSON.stringify(data, null, 2));
}

const server = http.createServer(async (req, res) => {
    const parsedUrl = url.parse(req.url, true);
    const path = parsedUrl.pathname;
    const method = req.method;
    
    console.log(`📡 ${method} ${path}`);
    
    // Handle CORS preflight requests
    if (method === 'OPTIONS') {
        res.writeHead(200, {
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE',
            'Access-Control-Allow-Headers': 'Content-Type'
        });
        res.end();
        return;
    }
    
    try {
        // 🏠 HOME PAGE
        if (method === 'GET' && path === '/') {
            res.writeHead(200, { 'Content-Type': 'text/html' });
            res.end(`
                <html>
                    <head>
                        <title>🍕 Tony's Pizza API</title>
                        <style>
                            body { font-family: Arial, sans-serif; margin: 40px; }
                            .endpoint { background: #f5f5f5; padding: 15px; margin: 10px 0; border-radius: 5px; }
                            .method { font-weight: bold; color: white; padding: 3px 8px; border-radius: 3px; }
                            .get { background-color: #61affe; }
                            .post { background-color: #49cc90; }
                            .put { background-color: #fca130; }
                            .delete { background-color: #f93e3e; }
                        </style>
                    </head>
                    <body>
                        <h1>🍕 Tony's Pizza Restaurant API</h1>
                        <p>Welcome to our RESTful Pizza API! Here are the available endpoints:</p>
                        
                        <h2>📋 Menu Endpoints</h2>
                        <div class="endpoint">
                            <span class="method get">GET</span> <code>/api/menu</code> - Get all pizzas
                        </div>
                        
                        <h2>🛒 Order Endpoints</h2>
                        <div class="endpoint">
                            <span class="method get">GET</span> <code>/api/orders</code> - Get all orders
                        </div>
                        <div class="endpoint">
                            <span class="method get">GET</span> <code>/api/orders/:id</code> - Get specific order
                        </div>
                        <div class="endpoint">
                            <span class="method post">POST</span> <code>/api/orders</code> - Create new order
                        </div>
                        <div class="endpoint">
                            <span class="method put">PUT</span> <code>/api/orders/:id</code> - Update order
                        </div>
                        <div class="endpoint">
                            <span class="method delete">DELETE</span> <code>/api/orders/:id</code> - Cancel order
                        </div>
                        
                        <h2>📊 Statistics</h2>
                        <div class="endpoint">
                            <span class="method get">GET</span> <code>/api/stats</code> - Get restaurant statistics
                        </div>
                        
                        <h2>🧪 Test the API</h2>
                        <p>You can test these endpoints using:</p>
                        <ul>
                            <li><strong>Browser:</strong> For GET requests, just click the links above</li>
                            <li><strong>Postman:</strong> For all HTTP methods</li>
                            <li><strong>curl:</strong> Command line tool</li>
                            <li><strong>JavaScript fetch():</strong> In browser console</li>
                        </ul>
                        
                        <h3>Quick Test Links:</h3>
                        <p><a href="/api/menu">📋 View Menu</a></p>
                        <p><a href="/api/orders">🛒 View Orders</a></p>
                        <p><a href="/api/stats">📊 View Statistics</a></p>
                    </body>
                </html>
            `);
            
        // 📋 GET MENU
        } else if (method === 'GET' && path === '/api/menu') {
            sendJSON(res, 200, {
                success: true,
                data: menu,
                message: 'Menu retrieved successfully'
            });
            
        // 🛒 GET ALL ORDERS
        } else if (method === 'GET' && path === '/api/orders') {
            sendJSON(res, 200, {
                success: true,
                data: orders,
                count: orders.length,
                message: 'Orders retrieved successfully'
            });
            
        // 🔍 GET SPECIFIC ORDER
        } else if (method === 'GET' && path.match(/^\/api\/orders\/\d+$/)) {
            const orderId = parseInt(path.split('/')[3]);
            const order = orders.find(o => o.id === orderId);
            
            if (order) {
                sendJSON(res, 200, {
                    success: true,
                    data: order,
                    message: `Order #${orderId} retrieved successfully`
                });
            } else {
                sendJSON(res, 404, {
                    success: false,
                    error: `Order #${orderId} not found`
                });
            }
            
        // 📝 CREATE NEW ORDER
        } else if (method === 'POST' && path === '/api/orders') {
            const orderData = await parseJSONBody(req);
            
            // Validate required fields
            if (!orderData.customerName || !orderData.pizzaId || !orderData.size) {
                sendJSON(res, 400, {
                    success: false,
                    error: 'Missing required fields: customerName, pizzaId, size'
                });
                return;
            }
            
            // Check if pizza exists
            const pizza = menu.find(p => p.id === orderData.pizzaId);
            if (!pizza) {
                sendJSON(res, 400, {
                    success: false,
                    error: 'Pizza not found in menu'
                });
                return;
            }
            
            // Create new order
            const newOrder = {
                id: orderIdCounter++,
                customerName: orderData.customerName,
                pizza: pizza,
                size: orderData.size,
                quantity: orderData.quantity || 1,
                status: 'received',
                orderTime: new Date().toISOString(),
                estimatedDelivery: new Date(Date.now() + 30 * 60 * 1000).toISOString() // 30 minutes
            };
            
            orders.push(newOrder);
            console.log('🎉 New order created:', newOrder);
            
            sendJSON(res, 201, {
                success: true,
                data: newOrder,
                message: `Order #${newOrder.id} created successfully`
            });
            
        // 📝 UPDATE ORDER
        } else if (method === 'PUT' && path.match(/^\/api\/orders\/\d+$/)) {
            const orderId = parseInt(path.split('/')[3]);
            const updateData = await parseJSONBody(req);
            
            const orderIndex = orders.findIndex(o => o.id === orderId);
            if (orderIndex === -1) {
                sendJSON(res, 404, {
                    success: false,
                    error: `Order #${orderId} not found`
                });
                return;
            }
            
            // Update order (only allow certain fields)
            const allowedFields = ['status', 'customerName', 'size', 'quantity'];
            const updatedOrder = { ...orders[orderIndex] };
            
            Object.keys(updateData).forEach(key => {
                if (allowedFields.includes(key)) {
                    updatedOrder[key] = updateData[key];
                }
            });
            
            updatedOrder.lastModified = new Date().toISOString();
            orders[orderIndex] = updatedOrder;
            
            console.log('📝 Order updated:', updatedOrder);
            
            sendJSON(res, 200, {
                success: true,
                data: updatedOrder,
                message: `Order #${orderId} updated successfully`
            });
            
        // 🗑️ DELETE ORDER
        } else if (method === 'DELETE' && path.match(/^\/api\/orders\/\d+$/)) {
            const orderId = parseInt(path.split('/')[3]);
            const orderIndex = orders.findIndex(o => o.id === orderId);
            
            if (orderIndex === -1) {
                sendJSON(res, 404, {
                    success: false,
                    error: `Order #${orderId} not found`
                });
                return;
            }
            
            const cancelledOrder = orders.splice(orderIndex, 1)[0];
            console.log('🗑️ Order cancelled:', cancelledOrder);
            
            sendJSON(res, 200, {
                success: true,
                data: cancelledOrder,
                message: `Order #${orderId} has been cancelled`
            });
            
        // 📊 GET STATISTICS
        } else if (method === 'GET' && path === '/api/stats') {
            const stats = {
                totalOrders: orders.length,
                ordersByStatus: {
                    received: orders.filter(o => o.status === 'received').length,
                    preparing: orders.filter(o => o.status === 'preparing').length,
                    ready: orders.filter(o => o.status === 'ready').length,
                    delivered: orders.filter(o => o.status === 'delivered').length
                },
                popularPizzas: menu.map(pizza => ({
                    name: pizza.name,
                    ordersCount: orders.filter(o => o.pizza.id === pizza.id).length
                })).sort((a, b) => b.ordersCount - a.ordersCount),
                totalRevenue: orders.reduce((sum, order) => 
                    sum + (order.pizza.price * order.quantity), 0
                ),
                averageOrderValue: orders.length > 0 ? 
                    orders.reduce((sum, order) => sum + (order.pizza.price * order.quantity), 0) / orders.length 
                    : 0
            };
            
            sendJSON(res, 200, {
                success: true,
                data: stats,
                message: 'Statistics retrieved successfully'
            });
            
        // 404 - NOT FOUND
        } else {
            sendJSON(res, 404, {
                success: false,
                error: 'Endpoint not found',
                availableEndpoints: [
                    'GET /',
                    'GET /api/menu',
                    'GET /api/orders',
                    'GET /api/orders/:id',
                    'POST /api/orders',
                    'PUT /api/orders/:id',
                    'DELETE /api/orders/:id',
                    'GET /api/stats'
                ]
            });
        }
        
    } catch (error) {
        console.error('💥 Server error:', error);
        sendJSON(res, 500, {
            success: false,
            error: 'Internal server error',
            message: error.message
        });
    }
});

server.listen(3000, () => {
    console.log('🍕 Tony\'s Pizza API is running at http://localhost:3000');
    console.log('📚 API Documentation: http://localhost:3000');
    console.log('\n🧪 Test with curl:');
    console.log('   curl http://localhost:3000/api/menu');
    console.log('   curl -X POST -H "Content-Type: application/json" -d \'{"customerName":"John","pizzaId":1,"size":"large"}\' http://localhost:3000/api/orders');
});
```

### Testing Your API 🧪

**1. Test with Browser (GET requests only):**
- Visit: `http://localhost:3000/api/menu`
- Visit: `http://localhost:3000/api/orders`

**2. Test with curl (all methods):**

```bash
# Get menu
curl http://localhost:3000/api/menu

# Create order
curl -X POST -H "Content-Type: application/json" \
-d '{"customerName":"Alice","pizzaId":1,"size":"large","quantity":2}' \
http://localhost:3000/api/orders

# Get all orders
curl http://localhost:3000/api/orders

# Get specific order
curl http://localhost:3000/api/orders/1

# Update order status
curl -X PUT -H "Content-Type: application/json" \
-d '{"status":"preparing"}' \
http://localhost:3000/api/orders/1

# Cancel order
curl -X DELETE http://localhost:3000/api/orders/1

# Get statistics
curl http://localhost:3000/api/stats
```

**3. Test with JavaScript (in browser console):**

```javascript
// Create an order
fetch('http://localhost:3000/api/orders', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        customerName: 'Sarah',
        pizzaId: 2,
        size: 'medium',
        quantity: 1
    })
})
.then(response => response.json())
.then(data => console.log('Order created:', data));

// Get all orders
fetch('http://localhost:3000/api/orders')
.then(response => response.json())
.then(data => console.log('Orders:', data));
```

## Advanced Server Concepts 🚀

### 1. Handling File Uploads

```javascript
// file-upload-server.js
const http = require('http');
const fs = require('fs');
const path = require('path');
const querystring = require('querystring');

const server = http.createServer((req, res) => {
    if (req.method === 'GET' && req.url === '/') {
        // Show upload form
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end(`
            <html>
                <body>
                    <h1>📁 File Upload Server</h1>
                    <form method="POST" action="/upload" enctype="multipart/form-data">
                        <p>
                            <label>Choose file:</label><br>
                            <input type="file" name="uploadFile" accept="image/*" required>
                        </p>
                        <p>
                            <label>Description:</label><br>
                            <input type="text" name="description" placeholder="Describe your file...">
                        </p>
                        <p>
                            <button type="submit">📤 Upload File</button>
                        </p>
                    </form>
                    
                    <h2>📂 Uploaded Files:</h2>
                    <div id="fileList">
                        ${getUploadedFilesList()}
                    </div>
                </body>
            </html>
        `);
        
    } else if (req.method === 'POST' && req.url === '/upload') {
        handleFileUpload(req, res);
        
    } else if (req.method === 'GET' && req.url.startsWith('/files/')) {
        // Serve uploaded files
        const fileName = req.url.split('/files/')[1];
        const filePath = path.join(__dirname, 'uploads', fileName);
        
        if (fs.existsSync(filePath)) {
            const fileStream = fs.createReadStream(filePath);
            res.writeHead(200, { 'Content-Type': 'application/octet-stream' });
            fileStream.pipe(res);
        } else {
            res.writeHead(404, { 'Content-Type': 'text/plain' });
            res.end('File not found');
        }
        
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Page not found');
    }
});

function handleFileUpload(req, res) {
    let body = '';
    req.on('data', chunk => {
        body += chunk.toString('binary');
    });
    
    req.on('end', () => {
        // Simple multipart parsing (use libraries like 'multiparty' in production!)
        const boundary = req.headers['content-type'].split('boundary=')[1];
        const parts = body.split('--' + boundary);
        
        parts.forEach(part => {
            if (part.includes('filename=')) {
                // Extract filename
                const filenameMatch = part.match(/filename="([^"]+)"/);
                if (filenameMatch) {
                    const filename = filenameMatch[1];
                    
                    // Extract file content (this is simplified - use proper libraries!)
                    const fileContent = part.split('\r\n\r\n')[1].split('\r\n--')[0];
                    
                    // Ensure uploads directory exists
                    if (!fs.existsSync('uploads')) {
                        fs.mkdirSync('uploads');
                    }
                    
                    // Save file
                    const filePath = path.join('uploads', filename);
                    fs.writeFileSync(filePath, fileContent, 'binary');
                    
                    console.log(`📁`)