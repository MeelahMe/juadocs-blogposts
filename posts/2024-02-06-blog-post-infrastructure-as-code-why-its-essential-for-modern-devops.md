#  Why Docs-as-Code Matters for Developer Experience

Originally published at [JuaDocs Blog](https://www.juadocs.com/blog/blog-post-title-two-stjld)

---
Manual infrastructure management doesn’t cut it anymore.
When you’re responsible for keeping critical systems running—like a national high-performance computing (HPC) data center—you need something more reliable, repeatable, and scalable.

During my time as a DevOps Engineering Intern at the National Energy Research Scientific Computing Center (NERSC), I worked on systems that handled real-time monitoring, tracked environmental sensors, and sent intelligent alerts. We used tools like OpenDCIM to validate hardware, automated system failure detection, and built 3D visualizations of resource usage. But what held everything together was Infrastructure-as-Code (IaC).

Codifying our infrastructure with tools like Ansible gave our team a shared language and a single source of truth. We could automate provisioning, ensure every server was set up exactly how it should be, and catch problems before they became outages. It helped us cut down on downtime, reduce energy waste, and save time chasing misconfigurations.

In this post, I’ll walk through why IaC—especially with Ansible—plays such a critical role in modern DevOps, and how to get started using it in a real-world environment.

This post explores why IaC—especially with Ansible—is essential to modern DevOps, and how it can help teams build resilient infrastructure from the ground up.

---

## What Is Infrastructure-as-Code?

nfrastructure-as-Code means managing your infrastructure the same way you manage your application code. Instead of logging into servers and making changes by hand, you define your infrastructure in configuration files that can be version-controlled, reviewed, and reused.

Ansible is one of the most approachable ways to do this. It uses simple, readable YAML files called playbooks to define how systems should be configured, what packages should be installed, and what services should be running. Since Ansible doesn’t require any agents and connects over SSH, it’s easy to introduce even in more traditional or hybrid environments.

What you get is consistency. No more wondering if a dev box was missing a setting or if prod was running a slightly different config. Every environment is built from the same playbook, so they behave the same way.

---

## Why Infrastructure-as-Code Is Essential for Modern DevOps

Modern DevOps is built on the principle of automation, and Infrastructure-as-Code is its foundation. As systems scale and teams grow, it becomes impossible to rely on tribal knowledge, one-off scripts, or undocumented server changes. IaC solves this by giving infrastructure the same rigor and repeatability as application code.

Here’s why it matters:

### 1. Consistency Across Environments
With Ansible, you can write one playbook and apply it across staging, production, or recovery systems. At NERSC, this meant we could validate that what worked in one rack of servers would behave the same in another—minimizing configuration drift and unexpected behavior.

### 2. Version Control and Collaboration
Infrastructure changes are tracked in Git. That means every adjustment—whether to a Kubernetes deployment or a data center monitoring script—is reviewed, documented, and revertible. As someone who collaborated across engineers and operations managers, this level of transparency ensured everyone stayed aligned.

### 3. Faster Recovery and Onboarding
When systems went down or needed to scale quickly, IaC allowed us to bring environments back online with minimal downtime. New engineers could get up to speed faster because infrastructure was readable, centralized, and well-documented.

### 4. Scalability and Sustainability
IaC isn’t just about speed—it’s also about building sustainable systems. At NERSC, infrastructure automation contributed to a more energy-efficient and financially sustainable data center. We weren’t guessing—we were provisioning based on well-defined code that reflected the real-world state of our systems.

---

## How to Get Started with Infrastructure-as-Code Using Ansible

Adopting Infrastructure-as-Code (IaC) with Ansible begins with more than installing tools—it starts with a mindset shift: treating infrastructure as a collaborative, testable, version-controlled asset. Ansible’s agentless, declarative approach makes it ideal for gradually integrating automation into existing infrastructure while keeping human readability and flexibility intact.

Here’s a detailed roadmap for starting strong with Ansible in production-minded environments:

### 1. Define a Small, Meaningful Use Case

Begin by selecting a manageable task suitable for automation, such as configuring a development environment or deploying a monitoring agent. Starting with a focused use case allows for incremental learning and minimizes potential disruptions.

### 2. Install Ansible and Set Up Your Control Node

**For Ubuntu:**

```bash
sudo apt update
sudo apt install ansible -y
```

**For RHEL/CentOS:**

```bash
sudo yum install epel-release -y
sudo yum install ansible -y
```

**For macOS (with Homebrew):**

```bash
brew install ansible
```

Confirm installation:

```bash
ansible --version
```

### 3. Create Your Inventory File
Define your infrastructure targets in a file like this:

```ini
[web]
web01 ansible_host=192.168.10.10 ansible_user=ubuntu

[monitoring]
prometheus01 ansible_host=192.168.10.20 ansible_user=devops
```

### 4. Create Modular, Reusable Playbooks

Structure your automation into reusable components:
```bash
ansible-galaxy init roles/prometheus
```

Within a role:
- `tasks/` – main setup logic
- `handlers/` – services to restart
- `templates/` – dynamic config files
- `defaults/` – default vars

### 5. Parameterize with Variables and Templates

Using variables makes your playbooks more flexible:
```yaml
global:
  scrape_interval: {{ scrape_interval }}

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: {{ prometheus_targets }}
```

### 6. Version Control Your Playbooks

Version control isn’t optional. Initialize a Git repo and commit your work:
```bash
git init
git add .
git commit -m "Initial Ansible setup"
```

### 7. Test Before Deployment

Use Ansible’s check mode to preview what changes would be made:
```bash
ansible-playbook -i inventory playbook.yml --check
```

Try tools like `ansible-lint`, `molecule`, or `vagrant` to validate roles.

### 8. Integrate into CI/CD Pipelines

Here’s a simple GitHub Actions workflow to lint or test Ansible playbooks:
```yaml
name: Ansible CI

on: [push]

jobs:
  ansible:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: sudo apt update && sudo apt install ansible -y
      - run: ansible-playbook -i inventory playbook.yml --check
```

### 9. Document Everything

- Include a `README.md` for every role or playbook
- Comment your YAML clearly
- Share diagrams or onboarding docs internally

---

Infrastructure-as-Code isn’t just a tooling trend—it’s a cultural and operational upgrade. With Ansible, teams can automate critical processes, eliminate guesswork, and ensure their infrastructure is as dependable as their codebase.

My experience at NERSC showed how automation and documentation work together to reduce risk and improve collaboration. Whether you're managing a startup’s cloud footprint or maintaining a supercomputing facility, Infrastructure-as-Code enables your infrastructure to scale with confidence—not chaos.

And Ansible is one of the most powerful tools to help you get there.

