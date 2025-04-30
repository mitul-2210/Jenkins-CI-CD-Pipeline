# CI/CD Pipeline for a Python App using Jenkins

## 1. Introduction

This project features a simple Python command-line tool integrated with a full-fledged CI/CD pipeline powered by Jenkins, Docker, and GitHub. The tool performs basic arithmetic (adding two numbers), while Jenkins automates the build, test, and deployment process.

> 💡 **Tip**: Ensure Docker Desktop is running before executing any Docker commands.

## 2. Project Components

### Directory Layout
```
.
├── sources/
│   ├── add2vals.py       # CLI interface
│   ├── calc.py           # Core logic for addition
│   └── test_calc.py      # Unit tests for the calculation
├── dist/                 # Compiled binary output
├── requirements.txt      # Project dependencies
├── Jenkinsfile           # CI/CD pipeline script
├── docker-compose.yml    # Container orchestration file
└── README.md             # Project documentation
```

### Core Files

- **add2vals.py**: Command-line tool that takes two values and prints their sum.
- **calc.py**: Contains the actual addition logic.
- **test_calc.py**: Unit tests ensuring the calculator logic works as expected.
- **requirements.txt**: Lists all necessary Python packages.

### Jenkins CI/CD Setup

- **Jenkinsfile**: Defines the automation pipeline.
- **docker-compose.yml**: Spins up Jenkins and related containers, configured with volumes and networking.

### Docker Infrastructure

- Jenkins Master: Handles the pipeline execution.
- Jenkins Agent: Executes pipeline steps in an isolated environment.
- Python Builder Container: Runs tests and builds the Python app.

---

## 3. Setup Instructions

### 🔧 Step 1: Launch Jenkins Environment

1. **Start Jenkins containers**:
   ```bash
   docker-compose up -d
   ```

<div align="center">
  <img src="/images/image1.jpg" alt="Start Jenkins containers">
</div>

2. **Retrieve the admin password**:
   ```bash
   docker-compose exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
   ```

<div align="center">
  <img src="/images/image2.png" alt="Admin password">
</div>

3. **Access Jenkins dashboard**:
   - Open your browser at `http://localhost:8080`
   - Paste the password to unlock Jenkins

<div align="center">
  <img src="/images/image3.png" alt="Login to Jenkins">
</div>

4. **Install suggested plugins** when prompted.

<div align="center">
  <img src="/images/image4.png" alt="Install plugins">
</div>

5. **Create your admin user** and continue.

7. **Set the Jenkins URL** (default is fine): `http://localhost:8080/`.
<div align="center">
  <img src="/images/image5.png" alt="Install plugins">
</div>


9. **Create a new pipeline job**:
   - Name: `simple-python-pyinstaller-app`
   - Type: Pipeline
   - In the configuration:
     - Use "Pipeline script from SCM"
     - SCM: Git
     - Repository URL: `https://github.com/mitul-2210/Jenkins-CI-CD-Pipeline.git`
     - Branch: `*/master`

<div align="center">
  <img src="/images/image6.png" alt="Pipeline config">
</div>

8. **Install Docker inside Jenkins**:
   ```bash
   docker-compose exec jenkins apt-get update && apt-get install -y docker.io
   docker-compose exec jenkins service docker start
   docker-compose exec jenkins docker --version
   ```

<div align="center">
  <img src="/images/image7.png" alt="Docker inside Jenkins">
</div>

9. **Add Docker plugins**:
   - Go to **Manage Jenkins → Plugins**
   - Install:
     - Docker Pipeline
     - Docker plugin
     - docker-build-step

<div align="center">
  <img src="/images/image8.png" alt="Plugin install">
</div>

10. **Restart Jenkins** to apply plugin changes:
   ```bash
   docker-compose restart jenkins
   ```

11. **Login again** and go to your pipeline.

12. **Trigger a build**:
   - Go to your pipeline project
   - Click **Build Now**

<div align="center">
  <img src="/images/image9.png" alt="Run pipeline">
</div>

---

### 🧪 Step 2: Test the Executable

1. **Download the build artifact**:
   - From the Jenkins build page, download the `add2vals` executable.

> Note: Since Jenkins runs in a Linux container, the output will be a Linux binary.

2. **Run the executable on Windows using WSL**:

```bash
# Confirm WSL is installed
wsl --version

# If not installed:
wsl --install

# After setup, open WSL terminal
cd /mnt/c/Users/<YourUsername>/Downloads

# Make the binary executable
chmod +x add2vals

# Execute the binary with two numbers
./add2vals 5 3
```

<div align="center">
  <img src="/images/image10.png" alt="Run in WSL">
</div>

---

## 4. Role of PyInstaller

**PyInstaller** bundles Python code, dependencies, and the interpreter into a single executable. Here's what it does behind the scenes:

1. **Analysis**:
   - Scans the Python entry script (`add2vals.py`)
   - Detects all required imports and packages

2. **Bundling**:
   - Packages everything, including the interpreter
   - Outputs a self-contained binary

3. **Delivery**:
   - Places the final executable in the `dist` folder
   - Ready for distribution without requiring Python installation

### 🔍 Why Use PyInstaller?

- **Portability**: Ship your app as a single file
- **No Dependency Headaches**: Everything is bundled
- **Cross-Platform**: Generate binaries for Windows, Linux, or macOS

---

## 5. Final Thoughts

This walkthrough showcased how to implement an end-to-end CI/CD workflow using Jenkins and Docker for a basic Python app. You’ve learned how to:

- Set up a Jenkins server and install necessary plugins
- Automate testing and packaging via a Jenkins pipeline
- Generate and run a standalone executable using PyInstaller

---

### 🙌 Thanks for Reading!

We hope this guide gave you a clear understanding of how CI/CD pipelines can streamline Python app deployment. Happy building!

---

Would you like me to turn this into a downloadable PDF or help add badges (e.g., build status) to the top of the README?
