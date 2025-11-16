# 📘 Student Management API with Custom Metrics (Spring Boot + Micrometer + Prometheus)

This project is a **RESTful Student Management System** built using **Spring Boot**, featuring full CRUD operations along with **custom application metrics** exposed to **Prometheus** using **Spring Boot Actuator + Micrometer**.

It demonstrates:

* Java Spring Boot development
* JPA/Hibernate database integration
* REST APIs for student operations
* Custom Micrometer counters and gauges
* Prometheus monitoring setup (Docker-compatible)

---

## 🚀 Features

### ✅ Student Management (CRUD)

* Add a new student
* Update a student
* Fetch all students
* Fetch student by ID
* Delete student

### 📊 Custom Metrics

* Custom Counter: `custom.metric.one`
* Custom Gauge: `custom.metric.two`
* Actuator + Micrometer metrics exposed at `/actuator/prometheus`
* Ready for Prometheus + Grafana dashboards

### 🐳 Docker-Friendly Monitoring

Prometheus config included for scraping metrics.

---

## 📂 Project Structure

```
src/main/java/com/example/demo
│
├── DemoApplication.java
├── entity/
│   └── Student.java
├── repository/
│   └── StudentRepository.java
└── controller/
    └── studentController.java
```

---

## 🧩 Entity Model

### `Student.java`

| Field      | Type    | Description         |
| ---------- | ------- | ------------------- |
| rollNo     | int     | Primary Key         |
| name       | String  | Student name        |
| percentage | float   | Academic percentage |
| branch     | String  | Department          |
| isPassed   | Boolean | Passed/Failed flag  |

---

## 🔧 Custom Metrics Implemented

### **1️⃣ Counter — Tracks total students created**

```java
this.myStudentCounter = Counter.builder("custom.metric.one")
    .tags("status", "created")
    .description("total number of students created - function one")
    .register(meterRegistry);
```

Incremented on every student creation:

```java
this.myStudentCounter.increment();
```

---

### **2️⃣ Gauge — Tracks total students present in DB**

```java
long val = repo.count();
Tags tags = Tags.of("status", "pending");
meterRegistry.gauge("custom.metric.two", tags, val);
```

---

## 📡 API Endpoints

### ➤ Get all students

```
GET /students
```

### ➤ Get student by ID

```
GET /students/{id}
```

### ➤ Add a new student

```
POST /students/add
```

Example Body:

```json
{
  "name": "Priya",
  "percentage": 88.5,
  "branch": "CSE",
  "isPassed": true
}
```

### ➤ Update an existing student

```
PUT /students/update/{id}
```

### ➤ Delete a student

```
DELETE /students/delete/{id}
```

---

## 📊 Prometheus Configuration

Add this inside **prometheus.yml**:

```yaml
global:
 scrape_interval: 15s
 evaluation_interval: 15s
 scrape_timeout: 10s

scrape_configs:
 - job_name: 'prometheus'
   static_configs:
     - targets: ['localhost:9090']

 - job_name: 'todo-api'
   metrics_path: '/actuator/prometheus'
   scrape_interval: 3s
   static_configs:
     - targets: ['host.docker.internal:8080']
       labels:
         application: 'spring-actuator'
```

---

## ▶️ Run the Project

### 1️⃣ Clone the repository

```bash
git clone https://github.com/priyagupta20044/Java-API-and-Custom-Metrics-using-Spring-Boot
```

### 2️⃣ Build

```bash
mvn clean install
```

### 3️⃣ Run

```bash
mvn spring-boot:run
```

---

## 🌐 URLs

| Type               | URL                                         |
| ------------------ | ------------------------------------------- |
| Application        | `http://localhost:8080`                     |
| Prometheus Metrics | `http://localhost:8080/actuator/prometheus` |

---

## 📈 Example Prometheus Metrics

```
custom_metric_one_total{status="created"} 5.0
custom_metric_two{status="pending"} 12.0
```


## 👩‍💻 Author

**Priya Gupta**
Java • Spring Boot • Machine Learning • Backend Development

