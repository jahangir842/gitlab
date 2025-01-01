### Guide: Enabling GitLab Authentication for SonarQube Users

This guide provides step-by-step instructions to integrate GitLab with SonarQube, allowing users to sign in or sign up on SonarQube using their GitLab credentials.

Video Tutorial: [GitLab Authentication for SonarQube](https://www.youtube.com/watch?v=XX0ey4rRvms)

---

### Prerequisites

1. A running **SonarQube instance** (Community, Developer, or Enterprise Edition).
2. Admin access to **GitLab**.
3. Admin access to **SonarQube**.
4. A GitLab **OAuth application** for integration.

---

### Step 1: Configure GitLab OAuth Application

1. Log in to your **GitLab** instance as an Admin.
2. Navigate to **Admin Area > Applications** (for self-managed GitLab) or **User Settings > Applications** (for GitLab.com users).
3. Click **New Application**.
4. Fill in the following details:
   - **Name:** Enter a name (e.g., `SonarQube Integration`).
   - **Redirect URI:** Use the following pattern, replacing `<SONARQUBE_URL>` with your SonarQube server's URL:
     ```
     <SONARQUBE_URL>/oauth2/callback/gitlab
     ```
     Example: `http://localhost:9000/oauth2/callback/gitlab`
   - **Scopes:** Select the following scopes:
     - `read_user`
     - `api` (optional, but required for advanced features like managing repositories).
5. Click **Save Application**.
6. Copy the **Application ID** and **Secret** values from the resulting screen. You’ll need these in the next step.

---

### Step 2: Configure SonarQube for GitLab Authentication

1. Log in to your **SonarQube** instance as an Admin.
2. Navigate to **Administration > Configuration > General Settings > Authentication > Gitlab > Create Configuration**.
3. Scroll down to **GitLab Authentication** and configure the following fields:
   - **GitLab URL:** 
     - For GitLab.com: `https://gitlab.com`
     - For self-hosted GitLab: Your GitLab server's URL (e.g., `https://gitlab.example.com`).
   - **Application ID:** Paste the Application ID from GitLab.
   - **Client Secret:** Paste the Secret from GitLab.
   - **Enabled:** Toggle this to `ON`.
4. Save the changes.

---

### Step 3: Test the Integration

1. Log out of SonarQube or open an incognito window.
2. On the SonarQube login page, you should see a new option: **Log in with GitLab**.
3. Click the button and sign in using a GitLab account.
4. Grant access when prompted by GitLab.
5. Verify that the GitLab user can successfully sign in to SonarQube.

---

### Step 4: Restrict Access (Optional)

If you want to restrict access to specific GitLab groups:

1. Navigate to **Administration > Configuration > General Settings > Security > Auth > GitLab** in SonarQube.
2. Find the **Allowed Groups** field and enter the group names you wish to allow (e.g., `dev-team`, `qa-team`).
3. Save the changes.

---

### Step 5: Automate User Permissions (Optional)

To automate assigning permissions based on GitLab groups:

1. Go to **Administration > Security > Groups** in SonarQube.
2. Create groups that match the GitLab group names (e.g., `dev-team`, `qa-team`).
3. Set the desired permissions for each group in SonarQube.
4. Users signing in via GitLab will automatically inherit these permissions.

---

### Troubleshooting Tips

1. **Login Button Missing:**
   - Ensure GitLab authentication is enabled in **SonarQube settings**.
   - Double-check the redirect URI format in GitLab and SonarQube.

2. **Failed to Authenticate:**
   - Verify the **Application ID** and **Secret** match in GitLab and SonarQube.
   - Ensure the GitLab scopes include at least `read_user`.

3. **Access Denied for Some Users:**
   - Check the **Allowed Groups** setting in SonarQube.
   - Verify that the users belong to the specified GitLab groups.

---

By following these steps, you’ll successfully enable GitLab authentication for your SonarQube instance, streamlining user access and management.
