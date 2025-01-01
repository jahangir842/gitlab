Here's a complete guide for registering and running a GitLab Runner on a Linux server in an **offline network** where the GitLab instance is also hosted.

---

### **Step 1: Install GitLab Runner**
1. **Download the GitLab Runner Binary**:
   - On the Linux server, download the GitLab Runner binary:
     ```bash
     curl -LO https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
     ```

2. **Install the Binary**:
   - Move the binary to `/usr/local/bin` and make it executable:
     ```bash
     sudo mv gitlab-runner-linux-amd64 /usr/local/bin/gitlab-runner
     sudo chmod +x /usr/local/bin/gitlab-runner
     ```

3. **Verify Installation**:
   ```bash
   gitlab-runner --version
   ```

---

### **Step 2: Register the Runner**
1. **Run the Registration Command**:
   - Use the `gitlab-runner register` command to register your runner with your offline GitLab instance:
     ```bash
     sudo gitlab-runner register
     ```

2. **Provide Required Details**:
   - **GitLab URL**: Enter the URL of your GitLab instance. For example:
     ```
     http://your-gitlab-instance.local
     ```
   - **Registration Token**: Obtain the token from your GitLab project or group:
     - Navigate to **Settings** → **CI/CD** → **Runners** → **Registration token**.
     - Paste the token here.
   - **Description**: Provide a name for the runner (e.g., `offline-linux-runner`).
   - **Tags**: Enter tags to identify the runner (e.g., `linux,offline`).
   - **Executor**: Choose the executor type (e.g., `shell`, `docker`, etc.). For example:
     ```
     shell
     ```

3. **Verify Registration**:
   - After registration, the configuration is stored in `/etc/gitlab-runner/config.toml`. Verify its content:
     ```bash
     sudo cat /etc/gitlab-runner/config.toml
     ```

---

### **Step 3: Start the GitLab Runner**
1. **Run GitLab Runner in the Background**:
   - Start the runner with:
     ```bash
     sudo gitlab-runner run
     ```
   - Keep it running in a separate session or terminal. Alternatively, configure it as a systemd service for automatic management.

2. **Configure GitLab Runner as a Service**:
   - Create a systemd service file:
     ```bash
     sudo nano /etc/systemd/system/gitlab-runner.service
     ```
   - Add the following content:
     ```ini
     [Unit]
     Description=GitLab Runner
     After=network.target

     [Service]
     ExecStart=/usr/local/bin/gitlab-runner run --working-directory /home/gitlab-runner
     Restart=always
     User=root

     [Install]
     WantedBy=multi-user.target
     ```

   - Enable and start the service:
     ```bash
     sudo systemctl enable gitlab-runner
     sudo systemctl start gitlab-runner
     ```

   - Verify the service status:
     ```bash
     sudo systemctl status gitlab-runner
     ```

---

### **Step 4: Verify Runner in GitLab**
1. Go to your GitLab project → **Settings** → **CI/CD** → **Runners**.
2. Check if your runner appears under **Available specific runners**.

---

### **Step 5: Use the Runner in `.gitlab-ci.yml`**
Create a `.gitlab-ci.yml` file in your project repository to use the runner. For example:

```yaml
stages:
  - build
  - test

build-job:
  stage: build
  tags:
    - offline
  script:
    - echo "Building the application..."
    - echo "This is an offline GitLab Runner."

test-job:
  stage: test
  tags:
    - offline
  script:
    - echo "Running tests..."
```

---

### **Optional Step: Update GitLab Runner**
Periodically update the runner manually to ensure compatibility with your GitLab instance:
1. Download the latest binary:
   ```bash
   curl -LO https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
   ```
2. Replace the old binary:
   ```bash
   sudo mv gitlab-runner-linux-amd64 /usr/local/bin/gitlab-runner
   sudo chmod +x /usr/local/bin/gitlab-runner
   ```

3. Restart the service:
   ```bash
   sudo systemctl restart gitlab-runner
   ```

---

### **Troubleshooting**
- **Runner Not Appearing in GitLab**:
  - Ensure the GitLab instance is reachable from the offline network.
  - Verify the token and GitLab URL used during registration.

- **CI/CD Jobs Fail**:
  - Check the logs:
    ```bash
    sudo journalctl -u gitlab-runner
    ```

This setup allows your offline Ubuntu server to function as a fully operational GitLab Runner in your isolated network.
