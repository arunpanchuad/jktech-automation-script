# 📚 Bookstore API Automation with CI/CD Integration

End-to-end API automation framework for a Bookstore API using Cucumber BDD, TestNG, and Java 18, with seamless CI/CD integration.

## 💻 Tech Stack

| Component | Description |
|-----------|-------------|
| 🧠 IDE | IntelliJ IDEA or any Java-compatible IDE |
| ☕ Language | Java 18 |
| 🔄 Framework | RestAssured + Cucumber BDD for behavior-driven API testing |
| 🛠 Build Tool | Maven for dependency management and CI/CD integration |
| ✅ Test Runner | TestNG for test execution, parallel runs, and retries |

## 🧪 Why This Stack?

- **TestNG**: Supports retries, parallel execution, and CI/CD integration, ideal for non-Spring Boot projects
- **Cucumber BDD**: Enables human-readable test cases for better collaboration between technical and non-technical stakeholders
- **RestAssured**: Simplifies API testing with a fluent interface and powerful assertion capabilities

## 📋 Prerequisites

### Java 18
- Ensure Java 18 is installed (Oracle JDK 18 or OpenJDK 18)
- Set `JAVA_HOME` to your Java 18 installation (e.g., `C:\Program Files\Java\jdk-18`)
- Add `%JAVA_HOME%\bin` to your system PATH
- Verify with: `java -version` (should show 18.0.1.1 or similar)

### Maven
- Download from [Apache Maven](https://maven.apache.org/download.cgi)
- Add Maven's bin directory to PATH
- Verify with: `mvn -version`

### Git
- Install from [Git for Windows](https://gitforwindows.org/) or equivalent for your OS
- Configure SSH keys for GitHub authentication (see [GitHub SSH setup](https://docs.github.com/en/authentication/connecting-to-github-with-ssh))
- Verify with: `git --version`

### Optional
- IntelliJ IDEA or Eclipse for easier project management
- Postman for manual API testing and validation

## 🚀 Setup and Running Test Cases

### 1. Clone the Repository

**Using SSH** (recommended if SSH keys are set up):
```bash
git clone git@github.com:arunpanchuad/jktech-automation-script.git
cd jktech-automation-script
```

**Using HTTPS** (alternative method):
```bash
git clone https://github.com/arunpanchuad/jktech-automation-script.git
cd jktech-automation-script
```

### 2. Verify Java Version
The project is configured for Java 18 (pom.xml sets `<maven.compiler.source>18</maven.compiler.source>`).

Confirm with:
```bash
java -version
```

### 3. Install Dependencies
Build the project to download dependencies:
```bash
mvn clean install
```

### 4. Run Test Cases
Execute all Cucumber tests via the CucumberRunner class:
```bash
mvn test
```

### API Endpoints Covered by Tests
- `POST /signup`: Sign up a new user
- `POST /login`: Login and generate a token
- `POST /books`: Create a new book
- `PUT /books/{id}`: Update a book
- `GET /books/{id}`: Fetch a book by ID
- `GET /books`: Fetch all books
- `DELETE /books/{id}`: Delete a book

## ⚠️ Troubleshooting

- **Test Failure**: If tests fail due to status line mismatches (e.g., `HTTP/1.1 422 Unprocessable Content`), ensure `UserStepDefs.java` asserts the correct status codes (e.g., 422). This has been fixed in the latest version.
- **Java Version Issue**: Verify `JAVA_HOME` points to Java 18 installation.
- **Debugging**: Run with debug logging:
  ```bash
  mvn test -e -X
  ```

## 🛠 CI/CD Integration

### Prerequisites
- **Jenkins**: Install locally with plugins (Git, GitHub, Pipeline, Maven)
- **Ngrok**: For exposing local Jenkins to GitHub webhooks (development environment only)

### Steps

#### 1. Add Jenkinsfile to Dev Repository
Create a Jenkinsfile to build Dev code and trigger QA automation:

```groovy
pipeline {
    agent any
    stages {
        stage('Build Dev') {
            steps {
                echo 'Building Dev code'
            }
        }
        stage('Trigger QA Automation') {
            steps {
                build job: 'QA-Repo'
            }
        }
    }
}
```

#### 2. Add Jenkinsfile to QA Repository
Create a Jenkinsfile to run tests:

```groovy
pipeline {
    agent any
    tools {
        maven 'Maven 3.6.3'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'git@github.com:arunpanchuad/jktech-automation-script.git', branch: 'master'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'mvn clean test'
            }
        }
    }
}
```

#### 3. Set Up Jenkins
- Launch Jenkins (`java -jar jenkins.war` or via installer)
- Install plugins: Git, GitHub, Pipeline, Maven
- Create two pipeline jobs: `Dev-Repo` and `QA-Repo`
- Configure each job to use the respective Jenkinsfile

#### 4. Configure Webhooks
- Use Ngrok to expose Jenkins:
  ```bash
  ngrok http http://localhost:8080
  ```
- Copy the Ngrok URL (e.g., `https://abc123.ngrok.io`)
- In the GitHub repository, add a webhook:
  - Payload URL: `https://gitUserName:gitPassword@<ngrok-url>/job/Dev-Repo/build`
  - Content type: `application/json`
  - Events: Push events

#### 5. Run CI/CD
- Commit changes to the repository
- The Dev job builds, then triggers the QA job to run tests

## 📝 Important Notes
- Ensure Java 18 is used consistently (`JAVA_HOME` and `pom.xml`)
- For production CI/CD, use a public Jenkins server instead of Ngrok
- SSH authentication is recommended for cloning; use HTTPS if SSH keys are not set up

---

## 📊 Test Coverage & Reporting
Test reports are available after execution in the `target/cucumber-reports` directory.