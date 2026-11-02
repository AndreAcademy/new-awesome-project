# Running Postman API Tests with GitHub Actions

## Overview

In this exercise, we will connect our Postman API tests with GitHub Actions.

The goal is to automatically run our API tests every time we push changes to GitHub. This is an example of **Continuous Integration (CI)**.

The workflow:

1. Create API tests in Postman
2. Export the Postman collection
3. Add the collection to GitHub
4. Create a GitHub Actions workflow
5. Automatically run API tests using Newman

---

# Step 1: Create API Tests in Postman

1. Open Postman.

2. Create a new collection:

   - Select **Collections**
   - Click **New Collection**
   - Give your collection a name

   Example:

   ```
   Student API Tests
   ```

3. Add API requests to your collection.

Example:

```
GET https://api.example.com/users
```

4. Add test scripts to validate the API response.

Example:

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

5. Run your collection in Postman and confirm that all tests pass.

---

# Step 2: Export the Postman Collection

GitHub Actions needs the Postman collection file to run the tests.

1. In Postman:

   - Open your collection
   - Click the three dots `(...)`
   - Select **Export**

2. Select:

```
Collection v2.1
```

3. Save the JSON file.

Example:

```
student-api-tests.json
```

---

# Step 3: Add the Collection to GitHub

Your repository should have a structure similar to:

```
My-Project
│
├── postman
│   └── student-api-tests.json
│
├── .github
│   └── workflows
│       └── api-tests.yml
│
└── README.md
```

The `postman` folder contains your Postman collections.

The `.github/workflows` folder contains your GitHub Actions workflows.

---

# Step 4: Create the GitHub Actions Workflow

Create a new file:

```
.github/workflows/api-tests.yml
```

Add the following:

```yaml
name: API Tests

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  postman-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Newman
        run: |
          npm install -g newman
          npm install -g newman-reporter-htmlextra

      - name: Run Postman Tests
        run: |
          newman run postman/student-api-tests.json \
          -r cli,htmlextra
```

---

# Step 5: Commit and Push Changes

Save your files and push them to GitHub.

Using Git:

```bash
git add .
git commit -m "Add Postman API tests pipeline"
git push
```

---

# Step 6: View the GitHub Actions Result

1. Open your GitHub repository.

2. Select:

```
Actions
```

3. Select:

```
API Tests
```

4. GitHub will start running your workflow.

A successful run will show:

```
✓ API Tests passed
```

A failed run will show:

```
✗ API Tests failed
```

Click the workflow run to view the detailed logs.

---

# Troubleshooting

## Collection file not found

Check that your YAML file matches the collection location:

```
postman/student-api-tests.json
```

The filename must exactly match the file stored in GitHub.

---

## Newman command not found

Check that Newman installation completed successfully:

```bash
npm install -g newman
```

---

## Tests pass in Postman but fail in GitHub Actions

Common causes:

- API URL is only available locally (`localhost`)
- Environment variables are missing
- Authentication tokens have expired

For real projects, sensitive values should be stored using **GitHub Secrets**.

---

# Learning Outcomes

After completing this exercise, you will understand how to:

✅ Create automated API tests in Postman  
✅ Store tests in GitHub  
✅ Run API tests automatically using GitHub Actions  
✅ Use Newman as a command-line API test runner  
✅ Understand the basics of Continuous Integration (CI)

This workflow is commonly used by QA Automation Engineers in modern software teams.