## GitLab Runner Setup (Self-Hosted GitLab)

### What is GitLab Runner?

**GitLab Runner** is an application that executes CI/CD jobs from GitLab pipelines. It picks up jobs defined in `.gitlab-ci.yml`, runs them in a specified executor (like Docker, shell, or virtual machines), and reports the results back to the GitLab server.

---

### Why Use GitLab Runner?

* Automates testing, building, and deployment of your code
* Integrates tightly with GitLab CI/CD pipelines
* Enables distributed and isolated job execution
* Supports multiple execution environments (Docker, shell, Kubernetes, etc.)
* Fully configurable to match infrastructure and performance requirements

Self-hosting a GitLab Runner allows fine-grained control over runners, job capacity, and execution security—ideal for secure, private, or large-scale environments.

---

## How It Works: Step-by-Step

To set up and connect a GitLab Runner to a self-hosted GitLab instance, the following steps are performed:

### Step 1: Install Required Packages

Basic system tools and APT transport packages are installed to ensure the runner can be downloaded and configured.

### Step 2: Add GitLab Runner Repository

* The GitLab Runner GPG key is imported.
* System architecture is detected to configure the correct APT repository.
* The GitLab Runner repository is added and the package index is updated.

### Step 3: Install GitLab Runner

The GitLab Runner package is installed from GitLab’s official repository.

### Step 4: Register the Runner

The runner is registered using the `gitlab-runner register` command with required parameters:

* GitLab instance URL
* Registration token
* Description
* Executor type (e.g., shell, docker)

This links the runner to the GitLab instance and prepares it to start accepting jobs.

### Step 5: Start and Enable GitLab Runner

The GitLab Runner systemd service is enabled and started to ensure it runs on system boot and is ready to process jobs.

---

## Running the Playbook

To install and register the GitLab Runner on a VM:

```bash
ansible-playbook playbooks/setup_gitlab_runner.yml
```

---

## Secure Your GitLab Tokens

Sensitive variables, such as your GitLab registration token and instance URL, should be stored in:

```bash
vars/gitlab_runner_vars.yml
```

Example values:

```yaml
gitlab_instance_url: "http://10.0.0.11"
gitlab_registration_token: "Token here"
```

To protect these credentials, **encrypt the file using Ansible Vault**:

```bash
ansible-vault encrypt vars/gitlab_runner_vars.yml
```

To edit later:

```bash
ansible-vault edit vars/gitlab_runner_vars.yml
```

To run the playbook securely:

```bash
ansible-playbook playbooks/setup_gitlab_runner.yml --ask-vault-pass
```

Or using a password file:

```bash
ansible-playbook playbooks/setup_gitlab_runner.yml --vault-password-file ~/.vault_pass.txt
```

<img width="770" alt="Screenshot 2025-05-26 at 7 39 57 AM" src="https://github.com/user-attachments/assets/e78657e9-3102-43b3-8d07-9bcd86e9d65c" />

