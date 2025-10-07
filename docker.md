Perfect 👌 — let’s make this a **hands-on Docker practice plan for Windows**, covering **all 4 experiments (9a–9d)** step-by-step.

You’ll be able to do this on your **Windows 10/11** system with **Docker Desktop** installed.
Each section is around **10–15 minutes** long, so the whole session is about **1 hour** if you do it continuously.

---

## 🧰 Prerequisites (Setup)

Before you start:

1. **Install Docker Desktop** from 👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. After installation, **restart your PC** once.
3. Open Docker Desktop → **Settings → General → Enable “Start Docker Desktop when you log in”**
4. Open **Command Prompt (cmd)** or **PowerShell** (Run as Administrator).
5. Verify installation:

   ```bash
   docker --version
   docker run hello-world
   ```

   ✅ If you see “Hello from Docker!”, you’re ready to go!

---

# 🧩 **EXPERIMENT 9a — Basic Docker Commands**

### 🕐 Time: 10 mins

### 🎯 Aim: Understand containers, images, and basic commands.

---

### 🔧 Commands to Try (in CMD)

```bash
# 1️⃣ Run the Hello World container
docker run hello-world

# 2️⃣ List all images
docker image ls

# 3️⃣ List all containers (active & inactive)
docker container ls -a

# 4️⃣ Rename a container (replace old_name with actual container name)
docker rename old_name intaa

# 5️⃣ Update memory and swap space for a container
docker update --memory 512m --memory-swap 1g intaa

# 6️⃣ Start a container
docker start intaa

# 7️⃣ Start interactively
docker start -i intaa

# 8️⃣ Open a Java shell inside Docker
docker run -it openjdk

# Inside jshell:
System.out.println("Hello from Docker!");
# Then press Ctrl + D to exit

# 9️⃣ Check container info
docker ps -a
```

✅ **Expected Output:**

* “Hello from Docker!” message from the hello-world and jshell containers.

---

# 🧩 **EXPERIMENT 9b — Build a Python Docker Image**

### 🕐 Time: 15 mins

### 🎯 Aim: Create a Python app and build it as a Docker image.

---

### 🔧 Steps

```bash
# 1️⃣ Create a new project folder
mkdir myapp
cd myapp

# 2️⃣ Create Python file
notepad app.py
```

📄 Inside **app.py**, write:

```python
print("Hello, this is docker container")
```

Save → Exit Notepad.

---

```bash
# 3️⃣ Create a Dockerfile
notepad Dockerfile
```

📄 Inside **Dockerfile**, paste:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir -r requirements.txt || true
CMD ["python", "app.py"]
```

Save → Exit Notepad.

---

```bash
# 4️⃣ Build image
docker build -t mypythonapp .

# 5️⃣ View images
docker images

# 6️⃣ Run container
docker run --name mycontainer mypythonapp

# 7️⃣ See containers
docker ps -a
```

✅ **Output:**
You’ll see: `Hello, this is docker container`

---

```bash
# 8️⃣ Modify Python file to run continuously
notepad app.py
```

Replace content with:

```python
import time
while True:
    print("Running inside Docker...")
    time.sleep(2)
```

Then rebuild and run:

```bash
docker build -t mypythonapp .
docker run --name mycontainer2 mypythonapp
```

Press **Ctrl+C** to stop.

```bash
docker stop mycontainer2
```

✅ **Output:**
Continuous “Running inside Docker…” messages.

---

# 🧩 **EXPERIMENT 9c — Simple Flask App in Docker**

### 🕐 Time: 15 mins

### 🎯 Aim: Run a small web app using Flask inside Docker.

---

### 🔧 Steps

```bash
mkdir myflaskapp
cd myflaskapp
notepad app.py
```

📄 Inside **app.py**, write:

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask running inside Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Save and close.

---

```bash
# 2️⃣ Create requirements.txt
notepad requirements.txt
```

📄 Inside it, just write:

```
flask
```

Save and close.

---

```bash
# 3️⃣ Create Dockerfile
notepad Dockerfile
```

📄 Paste:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

Save and close.

---

```bash
# 4️⃣ Build the image
docker build -t myflaskapp .

# 5️⃣ Run the container in detached mode
docker run -d -p 5000:5000 --name myflaskcontainer myflaskapp
```

✅ **Now open your browser:**
Go to 👉 [http://localhost:5000](http://localhost:5000)

You should see:

> “Hello from Flask running inside Docker!”

---

```bash
# 6️⃣ Stop & remove the container
docker stop myflaskcontainer
docker rm myflaskcontainer
```

✅ Flask app in Docker works!

---

# 🧩 **EXPERIMENT 9d — Managing Docker Containers and Volumes**

### 🕐 Time: 15 mins

### 🎯 Aim: Learn to use volumes (persistent storage) and manage containers/images.

---

### 🔧 Steps

```bash
mkdir myvolapp
cd myvolapp
notepad Dockerfile
```

📄 Inside **Dockerfile**, write:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . /app
CMD ["python", "-c", "open('/data/log.txt', 'w').write('Log written from Docker container')"]
```

Save and close.

---

```bash
# 2️⃣ Build image
docker build -t myvolapp .

# 3️⃣ Run container with volume mount
docker run -d -p 5006:5006 -v mydata:/data --name volcontainer myvolapp

# 4️⃣ List volumes
docker volume ls

# 5️⃣ Access the volume using alpine image
docker run --rm -it -v mydata:/data alpine cat /data/log.txt

# 6️⃣ Stop and remove container
docker stop volcontainer
docker rm volcontainer
```

✅ You should see:

> Log written from Docker container

---

## 🎓 Study / Viva Key Points

| Topic                | Example Command                      | Description              |
| -------------------- | ------------------------------------ | ------------------------ |
| Check Docker version | `docker --version`                   | Verify installation      |
| Pull image           | `docker pull image_name`             | Downloads image          |
| Run container        | `docker run image_name`              | Creates & runs container |
| List images          | `docker images`                      | Shows downloaded images  |
| List containers      | `docker ps -a`                       | Shows all containers     |
| Stop container       | `docker stop container_name`         | Stops it                 |
| Remove container     | `docker rm container_name`           | Deletes it               |
| Remove image         | `docker rmi image_name`              | Deletes image            |
| Build image          | `docker build -t name .`             | Builds from Dockerfile   |
| Run Flask in Docker  | `docker run -p 5000:5000 image_name` | Runs web app             |
| Volume mount         | `-v mydata:/data`                    | Persistent data mount    |

---

## ⚡ Quick 30-Minute Practice Workflow

| Time      | Action                                            |
| --------- | ------------------------------------------------- |
| 0–5 min   | Install & verify Docker with `hello-world`        |
| 5–15 min  | Create Python project and build Docker image (9b) |
| 15–25 min | Create Flask app and run it in browser (9c)       |
| 25–30 min | Add volume and verify log persistence (9d)        |

---

Would you like me to give you a **ZIP file template** (with all folders and code files prewritten — `myapp`, `myflaskapp`, `myvolapp`) so you can just unzip and run all the Docker commands immediately on your Windows machine?
