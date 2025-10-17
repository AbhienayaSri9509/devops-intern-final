##DevOps Intern Final Assessment

Name: Abhienaya Sri Date: 17-Oct-2025

##Description

This repository demonstrates a small DevOps workflow using Linux, GitHub, Docker, CI/CD, Nomad, and monitoring with Loki.

Instructions

Run scripts/sysinfo.sh for system info.

Build Docker container: docker build -t hello-devops .

Run container: docker run hello-devops

CI/CD pipeline runs automatically on GitHub push.

Deploy Nomad job: nomad job run nomad/hello.nomad

View logs in Loki as described in monitoring/loki_setup.txt

--- hello.py --- print("Hello, DevOps!")

--- scripts/sysinfo.sh --- #!/bin/bash whoami date df -h

--- Dockerfile --- FROM python:3.11-slim COPY hello.py /hello.py CMD ["python", "/hello.py"]

--- .github/workflows/ci.yml --- name: CI on: [push] jobs: build: runs-on: ubuntu-latest steps: - uses: actions/checkout@v3 - name: Run hello.py run: | python hello.py

--- nomad/hello.nomad --- job "hello" { datacenters = ["dc1"] type = "service"

group "hello-group" { task "hello-task" { driver = "docker"


Folder Structure:

```
devops-intern-final/
├── README.md
├── hello.py
├── scripts/
│   └── sysinfo.sh
├── Dockerfile
├── .github/
│   └── workflows/
│       └── ci.yml
├── nomad/
│   └── hello.nomad
├── monitoring/
│   └── loki_setup.txt
└── mlflow/  # Optional Advanced
    └── dummy_experiment.py
```

---

**README.md**

````markdown
# DevOps Intern Final Assessment

**Name:** Abhienaya Sri  
**Date:** 17-Oct-2025  

## Project Description
This project demonstrates a small DevOps workflow including Linux scripting, Docker containerization, CI/CD with GitHub Actions, Nomad deployment, and Loki monitoring.

## Folder Structure
- `scripts/` : Linux scripting basics
- `Dockerfile` : Docker container setup
- `.github/workflows/ci.yml` : CI/CD workflow
- `nomad/hello.nomad` : Nomad deployment
- `monitoring/loki_setup.txt` : Loki monitoring notes
- `mlflow/` : Optional MLFlow experiment

## How to Run
1. Run Linux script:
```bash
./scripts/sysinfo.sh
````

2. Build and run Docker:

```bash
docker build -t hello-devops .
docker run hello-devops
```

3. GitHub Actions will run `python hello.py` on every push.
4. Deploy with Nomad:

```bash
nomad job run nomad/hello.nomad
```

5. Loki logs: follow instructions in `monitoring/loki_setup.txt`

````

---

**hello.py**
```python
print("Hello, DevOps!")
````

**scripts/sysinfo.sh**

```bash
#!/bin/bash

echo "Current user: $(whoami)"
echo "Current date: $(date)"
echo "Disk usage:"
df -h
```

**Dockerfile**

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY hello.py .
CMD ["python", "hello.py"]
```

**.github/workflows/ci.yml**

```yaml
name: CI

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Run hello.py
        run: python hello.py
```

**nomad/hello.nomad**

```hcl
job "hello" {
  datacenters = ["dc1"]

  group "hello-group" {
    task "hello-task" {
      driver = "docker"
      config {
        image = "hello-devops"
      }
      resources {
        cpu    = 100
        memory = 128
      }
    }
  }
}
```

**monitoring/loki_setup.txt**

```
# Loki Setup Notes
1. Start Loki via Docker:
   docker run -d --name=loki -p 3100:3100 grafana/loki:2.8.2

2. To view logs from container:
   docker logs <container_id>

3. Alternatively, configure Nomad or Docker container log forwarding to Loki.
```

**mlflow/dummy_experiment.py** (Optional)

```python
import mlflow

mlflow.start_run()
mlflow.log_param("param1", 5)
mlflow.log_metric("metric1", 0.85)
mlflow.end_run()
```
