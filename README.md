require('dotenv').config();
const express = require('express');
const axios = require('axios');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

app.post('/api/send', async (req, res) => {
  const { email, name, tag } = req.body;

  try {
    const response = await axios.post('https://api.omnisend.com/v3/contacts', {
      email,
      name,
      tags: [tag]
    }, {
      headers: {
        'X-API-KEY': process.env.OMNISEND_API_KEY,
        'Content-Type': 'application/json'
      }
    });

    res.status(200).json({ success: true, data: response.data });
  } catch (error) {
    console.error('Omnisend API Error:', error.response?.data || error.message);
    res.status(500).json({ success: false, error: error.message });
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
# .gitignore
.env
{
  "name": "theface_form_trigger",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "axios": "^1.6.0",
    "body-parser": "^1.20.2",
    "dotenv": "^16.3.1",
    "express": "^4.18.2"
  }
}
