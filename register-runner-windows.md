### **Register Gitlab Runner with PowerShell on Windows**

---

#### **1. Install GitLab Runner**

1. **Download the GitLab Runner binary**:
   - Visit the [GitLab Runner Downloads page](https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe).
   - Save the binary to a directory, e.g., `C:\GitLab-Runner`.

2. **Rename the binary**:
   - Rename the downloaded file to `gitlab-runner.exe`.

---

#### **2. Register the Runner**

1. **Open a Command Prompt with administrative privileges**.
2. **Navigate to the directory** where `gitlab-runner.exe` is located:
   ```bash
   cd C:\GitLab-Runner
   ```
3. **Start the registration process**:
   ```bash
   gitlab-runner register
   ```
4. **Provide the required details during registration**:
   - **GitLab URL**: Enter the URL of your GitLab instance (e.g., `https://gitlab.com` or your self-hosted GitLab URL).
   - **Registration Token**: Get the token from your GitLab project:
     - Go to your GitLab project.
     - Navigate to **Settings > CI/CD > Runners > Specific Runners**.
     - Copy the token under **"Register a specific runner"**.
   - **Description**: Enter a description for the runner (e.g., "Windows Runner").
   - **Tags**: Enter tags to identify the runner (e.g., `windows`, `local`).
   - **Executor**: Choose **shell** as the executor for the Windows runner.

---

#### **3. Install and Run the Runner as a Service**

1. **Install the GitLab Runner as a Windows service**:
   ```bash
   gitlab-runner install
   ```
2. **Start the GitLab Runner service**:
   ```bash
   gitlab-runner start
   ```

---

#### **4. Verify the Runner**

1. Go back to your GitLab project.
2. Navigate to **Settings > CI/CD > Runners**.
3. You should see the runner listed under **"Available specific runners"**.

---

#### **5. Configure the Runner**

1. If needed, you can modify the runner configuration file located at:
   ```
   C:\GitLab-Runner\config.toml
   ```
2. You can adjust settings such as:
   - Executor type
   - Default tags
   - Environment variables

---

#### **6. Update `config.toml` to Use PowerShell**

1. **Edit the `config.toml` file**:
   - Change the shell to `powershell`:
     ```toml
     [[runners]]
       name = "Windows Runner"
       url = "https://gitlab.com/"
       token = "your_runner_token"
       executor = "shell"
       shell = "powershell"
     ```
2. **Restart the GitLab Runner**:
   - After saving the changes, restart the GitLab Runner service:
     ```bash
     gitlab-runner stop
     gitlab-runner start
     ```

---

#### **7. Example `.gitlab-ci.yml` for Windows Runner**

Once the runner is registered, you can use it in your `.gitlab-ci.yml` file by specifying the tags you assigned during registration.

```yaml
stages:
  - test

windows_job:
  stage: test
  tags:
    - windows
  script:
    - echo "Hello from Windows Runner!"
```

---

#### **8. Testing the Runner**

1. **Create a simple pipeline** by adding a `.gitlab-ci.yml` file to your project.
2. **Push the file to GitLab** to trigger the pipeline.
3. Verify the job runs on your Windows machine.

---

### **Summary**

- **Install GitLab Runner** by downloading the binary and setting it up.
- **Register the runner** with your GitLab project and configure it with PowerShell.
- **Configure and verify the runner** and ensure it’s listed as an available runner in your GitLab project.
- **Run a simple job** to verify everything is working properly.

This guide ensures your Windows machine is set up as a GitLab Runner using PowerShell, and you're ready to execute CI/CD pipelines!

---

Let me know if you need further assistance!
