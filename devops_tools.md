# Essential DevOps Tools on Linux

Beyond the core Linux commands, a set of specialized tools are indispensable for modern DevOps workflows. This guide provides an overview of key tools for version control, containerization, automation, and data manipulation.

### Table of Contents
1.  [Version Control](#1-version-control)
2.  [Containerization & Virtualization](#2-containerization--virtualization)
3.  [Infrastructure as Code (IaC) & Automation](#3-infrastructure-as-code-iac--automation)
4.  [Data Manipulation](#4-data-manipulation)
5.  [Terminal Productivity](#5-terminal-productivity)

---

## 1. Version Control

| Tool | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `git` | The de facto standard for distributed version control. Essential for managing code, configuration files, and documentation. | `git clone <repo_url>`: Copy a remote repository. <br> `git status`: Check the state of your working directory. <br> `git add .`: Stage all changes for the next commit. <br> `git commit -m "Your message"`: Record changes to the repository. <br> `git push`: Upload local commits to the remote repository. <br> `git pull`: Fetch and merge changes from the remote. |

---

## 2. Containerization & Virtualization

| Tool | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `docker` | The leading platform for developing, shipping, and running applications in containers. | `docker build -t my-app .`: Build an image from a Dockerfile. <br> `docker run -d -p 8080:80 my-app`: Run a container in **d**etached mode, mapping port 8080 to the container's port 80. <br> `docker ps`: List running containers. <br> `docker-compose up`: Build and start a multi-container application defined in `docker-compose.yml`. |
| `kubectl` | The command-line tool for interacting with a **Kube**rnetes cluster. | `kubectl get pods`: List all pods in the current namespace. <br> `kubectl apply -f deployment.yaml`: Create or update a resource from a file. <br> `kubectl logs <pod-name>`: View the logs from a pod. |

---

## 3. Infrastructure as Code (IaC) & Automation

| Tool | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| **Ansible** | An agentless automation tool for configuration management, application deployment, and task automation. Uses YAML playbooks. | `ansible-playbook -i hosts playbook.yml`: Run a playbook against a set of hosts. <br> `ansible all -m ping`: Ping all hosts in the inventory to check connectivity. |
| **Terraform** | A tool for building, changing, and versioning infrastructure safely and efficiently. Manages cloud services (AWS, GCP, Azure) via declarative configuration files. | `terraform init`: Initialize a working directory. <br> `terraform plan`: Create an execution plan. <br> `terraform apply`: Apply the changes required to reach the desired state. |

---

## 4. Data Manipulation

| Tool | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `jq` | A lightweight and flexible command-line JSON processor. Invaluable for working with APIs. | `curl ... | jq '.items[0].name'`: Extract the `name` field from the first item in a JSON array. <br> `kubectl get pods -o json | jq '.items[] | {name:.metadata.name, status:.status.phase}'`: Prettify `kubectl` output. |
| `yq` | A command-line YAML processor, similar to `jq`. | `yq '.services.web.ports[0]' docker-compose.yml`: Read a value from a YAML file. |

---

## 5. Terminal Productivity

| Tool | Purpose | Common Usage / Example |
| :--- | :--- | :--- |
| `tmux` | A **t**erminal **mu**ltiple**x**er. It lets you create and manage multiple terminal sessions from a single window, and keeps them running even if you disconnect. | `tmux new -s my_session`: Start a new named session. <br> `Ctrl+b, d`: Detach from the current session. <br> `tmux attach -t my_session`: Re-attach to a session. <br> `Ctrl+b, %`: Split the pane vertically. <br> `Ctrl+b, "`: Split the pane horizontally. |
| `screen` | An older, but still widely used, terminal multiplexer. | `screen -S my_session`: Start a new named session. <br> `Ctrl+a, d`: Detach from the session. <br> `screen -r my_session`: Resume a session. |
