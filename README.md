# Jenkins CI/CD Pipeline with Docker and AWS Deployment

This project demonstrates a full CI/CD pipeline using **Jenkins**, **Docker**, **AWS ECR**, and **EC2**.

## 🔧 Stack

- GitHub (source repo)
- Jenkins (automation)
- Docker (build container)
- Amazon ECR (Docker image registry)
- EC2 (deployment target)

## ⚙️ Pipeline Flow

1. ✅ Clone code from GitHub
2. 🛠️ Build Docker image with `BUILD_ID` tag
3. 🔐 Authenticate and push image to AWS ECR
4. 🚀 SSH into EC2 instance and deploy container

## 📂 Project Structure

```plaintext
.
├── Jenkinsfile
├── docker-practice/
│   ├── Dockerfile
│   └── app source files...
└── README.md
```



## 🖼️ Architecture Diagram

![GitHub](https://github.com/user-attachments/assets/9eec4da9-c4f4-4b30-946f-9b6e130396bb)

## 💻 Example Jenkinsfile Snippet

```groovy
stage('Build Docker Image') {
    steps {
        sh "docker build -t awesome-app:${env.BUILD_ID} docker-practice"
    }
}
```

## 🚀 Deployment Target

- **Environment:** EC2 instance (Ubuntu)
- **Deployment Port:** 80 (container exposed on port 80)

---

## ✅ Final Notes

This project was built to strengthen my DevOps skills by implementing a real-world, end-to-end CI/CD pipeline using Jenkins, Docker, and AWS services.

Feel free to explore, clone, or fork the repo — feedback and collaboration are welcome!

