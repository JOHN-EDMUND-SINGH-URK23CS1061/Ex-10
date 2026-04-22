# 🧪 Experiment 3: Monolithic Application using Flask

## 🎯 Aim

To develop and demonstrate a simple monolithic application using Python and Flask.

---

## 📖 Theory

A **monolithic application** is a single-tier application where all components (UI, business logic, and data) are combined into one unit.

---

## 🛠️ Step 1: Install Flask

```bash
pip install flask
```

---

## 📄 Step 2: Create Flask Application (app.py)

```python
from flask import Flask, request, jsonify

app = Flask(__name__)
students = {}
next_id = 1

@app.route('/students', methods=['GET'])
def get_students():
    return jsonify(list(students.values()))

@app.route('/students', methods=['POST'])
def add_student():
    global next_id
    data = request.get_json()
    student = {'id': next_id, 'name': data['name'], 'roll': data['roll']}
    students[next_id] = student
    next_id += 1
    return jsonify(student), 201

@app.route('/students/<int:sid>', methods=['DELETE'])
def delete_student(sid):
    students.pop(sid, None)
    return jsonify({'msg': 'Deleted'})

if __name__ == '__main__':
    app.run(debug=True)
```

---

## ▶️ Step 3: Run Application

```bash
python app.py
```

👉 Open: http://localhost:5000/students

---

## 🧪 Step 4: Test using PowerShell

### 🔹 Add Student

```powershell
Invoke-RestMethod -Uri "http://localhost:5000/students" `
-Method POST `
-Headers @{"Content-Type"="application/json"} `
-Body '{"name":"Alice","roll":"CS001"}'
```

---

### 🔹 Get All Students

```powershell
Invoke-RestMethod -Uri "http://localhost:5000/students" -Method GET
```

---

### 🔹 Delete Student

```powershell
Invoke-RestMethod -Uri "http://localhost:5000/students/1" -Method DELETE
```

---

## ✅ Expected Output

* Student added successfully
* Student list displayed
* Student deleted successfully

---

## ⚠️ Common Errors

* ❌ Using `/` instead of `/students` (causes 404)
* ❌ Wrong JSON format
* ❌ Flask not installed

---

## 🧠 Viva Questions

**1. What is a monolithic application?**
A single unified application where all components are combined.

**2. Advantage?**
Simple to develop and deploy.

**3. Disadvantage?**
Difficult to scale and maintain.

**4. What is REST API?**
API using HTTP methods like GET, POST, DELETE.

**5. What is Flask?**
A lightweight Python web framework.
