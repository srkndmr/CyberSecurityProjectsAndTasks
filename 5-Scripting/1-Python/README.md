# Contact Form Project

## **Overview** ✉️
This project demonstrates the implementation of a contact form using the Flask framework in Python. It covers essential concepts like form sanitization, validation, server-side processing, and rendering feedback to the user. The project adheres to secure practices and aims to teach backend form handling fundamentals.

---

## **Project Features**
- A fully functional contact form.
- Input sanitization to neutralize potentially harmful content.
- Server-side validation for required fields and email format.
- Pre-filled fields for valid inputs when errors occur.
- Radio buttons for single-choice selection.
- A confirmation page displaying submitted information upon success.
- Implemented with Flask templates and Jinja2.

---

## **Folder Structure** 🗂️
```
contact_form_project/
│
├── app.py                   # Main application file (backend logic)
├── templates/
│   ├── index.html           # Contact form page
│   ├── thank_you.html       # Thank you page
```

---

## **How It Works**

### **Backend (`app.py`):**
The Flask application handles the backend logic, including routing, processing form submissions, and rendering templates.

#### **Routes:**
- **`/`:** Displays the contact form and handles form submission.
- **`/thank_you`:** Displays a summary of the user’s submitted data upon successful validation.

#### **Code Highlights:**
```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    errors = {}
    data = {}

    if request.method == 'POST':
        data = request.form.to_dict()
        
        if not data.get('first_name'):
            errors['first_name'] = "First name is required."
        if not data.get('last_name'):
            errors['last_name'] = "Last name is required."
        if not data.get('email') or '@' not in data['email']:
            errors['email'] = "A valid email is required."
        if not data.get('country'):
            errors['country'] = "Please select a country."
        if not data.get('gender'):
            errors['gender'] = "Gender is required."
        if not data.get('subject'):
            errors['subject'] = "Please select a subject."
        if not data.get('message'):
            errors['message'] = "Message is required."

        if errors:
            return render_template('index.html', errors=errors, data=data)
        else:
            return render_template('thank_you.html', data=data)

    return render_template('index.html', errors={}, data={})

if __name__ == '__main__':
    app.run(debug=True)
```

---

### **Frontend (`index.html`):**
The contact form collects user data, validates inputs, and displays error messages.

#### **HTML Highlights:**
```html
<h1>Contact Technical Support</h1>
<form method="POST">
    <label for="first_name">First Name:</label>
    <input type="text" id="first_name" name="first_name" value="{{ data.get('first_name', '') }}">
    {% if errors.get('first_name') %}
        <span style="color: red;">{{ errors['first_name'] }}</span>
    {% endif %}

    <label for="last_name">Last Name:</label>
    <input type="text" id="last_name" name="last_name" value="{{ data.get('last_name', '') }}">
    {% if errors.get('last_name') %}
        <span style="color: red;">{{ errors['last_name'] }}</span>
    {% endif %}

    <label for="email">Email:</label>
    <input type="text" id="email" name="email" value="{{ data.get('email', '') }}">
    {% if errors.get('email') %}
        <span style="color: red;">{{ errors['email'] }}</span>
    {% endif %}

    <label for="gender">Gender:</label>
    <label><input type="radio" name="gender" value="Male" {% if data.get('gender') == 'Male' %}checked{% endif %}> Male</label>
    <label><input type="radio" name="gender" value="Female" {% if data.get('gender') == 'Female' %}checked{% endif %}> Female</label>
    {% if errors.get('gender') %}
        <span style="color: red;">{{ errors['gender'] }}</span>
    {% endif %}

    <label for="country">Country:</label>
    <select id="country" name="country">
        <option value="">Select a country</option>
        <option value="USA" {% if data.get('country') == 'USA' %}selected{% endif %}>USA</option>
        <option value="Canada" {% if data.get('country') == 'Canada' %}selected{% endif %}>Canada</option>
    </select>
    {% if errors.get('country') %}
        <span style="color: red;">{{ errors['country'] }}</span>
    {% endif %}

    <label>Subject:</label>
    <label><input type="radio" name="subject" value="Repair" {% if data.get('subject') == 'Repair' %}checked{% endif %}> Repair</label>
    <label><input type="radio" name="subject" value="Order" {% if data.get('subject') == 'Order' %}checked{% endif %}> Order</label>
    <label><input type="radio" name="subject" value="Others" {% if data.get('subject') == 'Others' %}checked{% endif %}> Others</label>
    {% if errors.get('subject') %}
        <span style="color: red;">{{ errors['subject'] }}</span>
    {% endif %}

    <label for="message">Message:</label>
    <textarea id="message" name="message">{{ data.get('message', '') }}</textarea>
    {% if errors.get('message') %}
        <span style="color: red;">{{ errors['message'] }}</span>
    {% endif %}

    <button type="submit">Submit</button>
</form>
```

---

### **Thank You Page (`thank_you.html`):**
Displays submitted data upon successful validation.

#### **HTML Highlights:**
```html
<h1>Thank You for Contacting Us!</h1>
<p>Here is the information you submitted:</p>
<ul>
    <li><strong>First Name:</strong> {{ data['first_name'] }}</li>
    <li><strong>Last Name:</strong> {{ data['last_name'] }}</li>
    <li><strong>Email:</strong> {{ data['email'] }}</li>
    <li><strong>Country:</strong> {{ data['country'] }}</li>
    <li><strong>Gender:</strong> {{ data['gender'] }}</li>
    <li><strong>Subject:</strong> {{ data['subject'] }}</li>
    <li><strong>Message:</strong> {{ data['message'] }}</li>
</ul>
```

---

## **Key Concepts Covered**
- **Flask Framework:** Used for routing and request handling.
- **Input Validation:** Ensures fields are correctly filled.
- **Jinja2 Templating:** Dynamic rendering of templates with user input.
- **Server-Side Sanitization:** Neutralizes harmful input like `<script>` tags.

---

## **How to Run the Project**
1. Clone this repository.
2. Set up a virtual environment:
   ```bash
   python3 -m venv myenv
   source myenv/bin/activate
   ```
3. Install Flask:
   ```bash
   pip install flask
   ```
4. Run the application:
   ```bash
   python app.py
   ```
5. Access the app at `http://127.0.0.1:5000` in your browser.

---

Enjoy building secure and dynamic web applications with Flask!

