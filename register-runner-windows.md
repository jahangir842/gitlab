### 1. **Install GitLab Runner**
1. Download the GitLab Runner binary for Windows from the [official GitLab Runner page](https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe).
2. Save it to a directory, e.g., `C:\GitLab-Runner`.
3. Rename the binary to `gitlab-runner.exe`.

---

### 2. **Register the Runner**
1. Open a Command Prompt with administrative privileges.
2. Navigate to the directory where `gitlab-runner.exe` is located:
   ```bash
   cd C:\GitLab-Runner
   ```
3. Run the following command to start the registration process:
   ```bash
   gitlab-runner register
   ```

4. Provide the required details during registration:
   - **GitLab URL:** Provide the URL of your GitLab instance (e.g., `https://gitlab.com` or your self-hosted GitLab URL).
   - **Registration Token:** Get the token from your GitLab project:
     - Go to your GitLab project.
     - Navigate to **Settings > CI/CD > Runners > Specific Runners**.
     - Copy the token under "Register a specific runner."
   - **Description:** Enter a description for the runner (e.g., `Windows Runner`).
   - **Tags:** Enter tags to identify the runner (e.g., `windows, local`).
   - **Executor:** Choose `shell` as the executor for a Windows runner.

---

### 3. **Install and Run the Runner as a Service**
1. Install the GitLab Runner as a Windows service by running:
   ```bash
   gitlab-runner install
   ```
2. Start the service:
   ```bash
   gitlab-runner start
   ```

---

### 4. **Verify the Runner**
- Go back to your GitLab project.
- Navigate to **Settings > CI/CD > Runners**.
- You should see the runner listed under "Available specific runners."

---

### 5. **Configure the Runner**
If needed, modify the runner configuration file located at:
```
C:\GitLab-Runner\config.toml
```
You can adjust settings such as executor type, default tags, or environment variables.

---

### Example `.gitlab-ci.yml` for Windows Runner
Once the runner is registered, you can use it in your `.gitlab-ci.yml` file by specifying the tags you assigned during registration:

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

Now your Windows machine is set up as a GitLab Runner and ready to execute pipelines!
