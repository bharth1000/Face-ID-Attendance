# 🎓 Face-ID Attendance (Serverless Project)

A serverless attendance system built using **AWS Rekognition, Lambda, DynamoDB, API Gateway, and S3**.  
Students register once with their face, and later mark attendance by verifying with a live selfie.

---

## 🚀 Features
- 📸 Face registration with student details
- ✅ Attendance marking using AWS Rekognition face match
- ☁️ Fully serverless (no backend servers needed)
- 🗄️ Attendance records stored in DynamoDB
- 🌐 API powered by AWS API Gateway + Lambda
- 🎨 Modern responsive UI with TailwindCSS
- 🔒 Secure (CORS enabled, S3 static hosting)

---

## 🛠️ Tech Stack
- **Frontend**: HTML, CSS (Tailwind), JavaScript  
- **AWS Services**:
  - S3 (static website hosting, image storage)
  - DynamoDB (attendance database)
  - Rekognition (face detection & comparison)
  - Lambda (business logic)
  - API Gateway (REST APIs for Register & Attendance)
  - IAM (secure roles & permissions)

---

## 📂 Project Structure
```
📁 FaceID-Attendance
 ├── index.html   # Main frontend (Register & Attendance UI)
 ├── README.md    # Documentation
 └── /lambda      # AWS Lambda functions
      ├── RegisterStudent.py
      └── MarkAttendance.py
```

---

## ⚙️ Setup Instructions

### 1. Create an S3 Bucket
- Enable **static website hosting**.  
- Upload `index.html` as the frontend.  
- Allow public access or use CloudFront for secure hosting.

### 2. Setup DynamoDB
- Create table: **AttendanceTable**  
  - Partition key: `StudentID` (String)  
  - Sort key (optional): `Timestamp`  

### 3. Setup Rekognition
- Create a face collection:  
  ```bash
  aws rekognition create-collection --collection-id StudentFacesCollection
  ```

### 4. IAM Role
- Create an IAM role with permissions for:
  - S3 (PutObject, GetObject)
  - Rekognition (IndexFaces, SearchFacesByImage)
  - DynamoDB (PutItem, GetItem)
  - Attach this role to all Lambda functions.

### 5. Lambda Functions
- **Register Student**
  - Uploads student face image to S3
  - Indexes face in Rekognition
  - Saves details in DynamoDB
- **Mark Attendance**
  - Captures selfie
  - Matches face against Rekognition collection
  - Marks attendance as **Present** or **Absent** in DynamoDB

### Example: Test Event (Register Student)
```json
{
  "student_id": "23CSE1024",
  "student_name": "Bharath M",
  "class": "B.E CSE",
  "section": "A",
  "roll": "1024",
  "phone": "9876543210",
  "image": "<base64-image>"
}
```

### Example: Test Event (Mark Attendance)
```json
{
  "student_id": "23CSE1024",
  "student_name": "Bharath M",
  "image": "<base64-image>"
}
```

### 6. API Gateway
- Create a **REST API**.  
- Endpoints:
  - `POST /register` → RegisterStudent Lambda
  - `POST /attendance` → MarkAttendance Lambda  
- Enable **CORS**.  
- Deploy API and copy the base URL.

### 7. Update Frontend
In `index.html`, update API URLs:
```js
const REGISTER_API = "https://<api-id>.execute-api.<region>.amazonaws.com/prod/register";
const ATTEND_API   = "https://<api-id>.execute-api.<region>.amazonaws.com/prod/attendance";
```

---

## 🎨 UI Preview
- Sky-blue + White modern design
- Circle camera view
- Glow buttons with hover effects
- Separate pages for:
  - 📝 Registration
  - ✅ Attendance

---

## 📌 Future Enhancements
- Admin dashboard for attendance reports
- SMS/email notifications for parents
- Multi-class support
- Biometric + Face ID hybrid attendance

---

## 👨‍💻 Author
**Bharath M**  
📍 Coimbatore, India  
Serverless | Cloud | AWS Enthusiast
