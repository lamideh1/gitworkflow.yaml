# gitworkflow.yaml
Sure, here’s a **step-by-step guide** on how to implement a complete GitHub Actions CI workflow that builds, tests, and uses best practices like environment variables, secrets, conditional execution, and matrix builds.

---

## **Step-by-Step: Setting Up GitHub Actions for CI**

---

### **Step 1: Prepare Your Project**

Make sure your project:

* Is a Node.js app (or any app with a `package.json`).
* Has scripts defined in `package.json`:

  ```json
  {
    "scripts": {
      "build": "your-build-command",
      "test": "your-test-command"
    }
  }
  ```

---

### **Step 2: Create GitHub Actions Workflow File**

In your project root:

1. Create the folder:

   ```bash
   mkdir -p .github/workflows
   ```

2. Create the workflow file:

   ```bash
   touch .github/workflows/main.yml
   ```

---

### **Step 3: Write Your Workflow (main.yml)**

Paste the following:

```yaml
name: CI Pipeline

env:
  GLOBAL_VAR: "This is a global environment variable"

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [14, 16, 18]  # Runs job for Node.js v14, 16, 18

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install Dependencies
        run: npm install

      - name: Build App
        run: npm run build

      - name: Run Tests
        run: npm test

  deploy:
    needs: build-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'

    steps:
      - name: Use Secret Token
        run: echo "Deploying with token: ${{ secrets.ACCESS_TOKEN }}"
```

---

### **Step 4: Set Secrets in GitHub**

1. Go to your GitHub repository.
2. Navigate to **Settings > Secrets and variables > Actions > Secrets**.
3. Click **New repository secret**.
4. Add your secret, e.g.:

   * Name: `ACCESS_TOKEN`
   * Value: your token string

---

### **Step 5: Commit and Push**

```bash
git add .github/workflows/main.yml
git commit -m "Add CI workflow with build, test, deploy"
git push origin main
```

GitHub Actions will automatically trigger and start running your workflow.

---

### Optional: Debugging Tips

* Check workflow runs under **Actions** tab in your repository.
* Use `echo` to print debug info in steps.
* Use `fail-fast: false` under `strategy` to allow all matrix jobs to run even if one fails.

---

Would you like me to generate a customized workflow based on your actual project stack or give you a sample repo to fork and test?
