# IP Address Collector

This repository contains a guide and example code for setting up a system to collect and store IP addresses from users. It provides both frontend and backend components, demonstrating how to capture IP information safely and efficiently.

## Prerequisites

- Node.js and npm
- MongoDB for storing IP addresses
- Basic knowledge of web development

## Installation

### Backend Setup

1. **Create a new Node.js project**:
   ```bash
   mkdir ip-collector-backend
   cd ip-collector-backend
   npm init -y
   npm install express mongoose body-parser cors dotenv
   ```

2. **Setup MongoDB**:
   - Ensure MongoDB is installed and running on your machine or use MongoDB Atlas for a cloud-based solution.

3. **Create server file**:
   - `server.js`:
     ```javascript
     const express = require('express');
     const bodyParser = require('body-parser');
     const cors = require('cors');
     const mongoose = require('mongoose');

     const app = express();

     // Middleware
     app.use(cors());
     app.use(bodyParser.json());

     // MongoDB connection
     mongoose.connect('mongodb://localhost/ip-collector', {
       useNewUrlParser: true,
       useUnifiedTopology: true
     });

     // Model
     const IpAddress = mongoose.model('IpAddress', new mongoose.Schema({
       ip: String
     }));

     // Routes
     app.post('/ip', (req, res) => {
       const newIp = new IpAddress({ ip: req.body.ip });
       newIp.save().then(() => res.json({ message: 'IP saved!' }));
     });

     const PORT = process.env.PORT || 3000;
     app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
     ```

### Frontend Setup

1. **Simple HTML page to capture IP**:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Capture IP</title>
   </head>
   <body>
     <h1>IP Address Collector</h1>
     <script>
       fetch('https://api.ipify.org?format=json')
         .then(response => response.json())
         .then(data => {
           fetch('http://localhost:3000/ip', {
             method: 'POST',
             headers: {
               'Content-Type': 'application/json'
             },
             body: JSON.stringify({ ip: data.ip })
           });
         });
     </script>
   </body>
   </html>
   ```

## Contributing

Contributions to enhance functionality, improve the setup guide, or provide additional security measures are welcome. Please fork the repository, make your changes, and submit a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.
