# CI/CD Pipeline Implementation for React Vite Project

This repository contains the configuration and documentation for a Continuous Integration/Continuous Deployment (CI/CD) pipeline, which is used to automate the build, test, and deployment process for a React Vite project.

## Project Overview

This project is designed to deploy a React application using GitHub Actions, automating the CI/CD pipeline to GitHub Pages. The pipeline handles the build and deployment process, ensuring that every push to the `master` branch triggers an automated workflow.

## Pipeline Configuration

### Workflow File

The pipeline is configured using GitHub Actions, with the workflow file located at `.github/workflows/deploy.yaml`. This file contains all the steps necessary to build and deploy the project.

```yaml
name: Deploy

on:
  push:
    branches:
      - master

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v4
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/master'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```
### Key Features
- Automated Build: The pipeline automatically builds the project on every push to the master branch.
- Artifact Management: The build files are uploaded as artifacts and used in the deployment step, ensuring that only built files are deployed.
- Deployment to GitHub Pages: The project is deployed to GitHub Pages, making it available as a static site.
- 
### Setting Up the Pipeline
#### Prerequisites
- Ensure you have a GitHub repository with a React Vite project.
- GitHub Pages should be enabled in your repository settings, with the source set to the gh-pages branch.
- GitHub Actions should have the necessary permissions to read and write to your repository.

### Steps to Set Up
1- Create the Workflow File:
  - Navigate to your GitHub repository.
  - Create a directory: .github/workflows/.
  - Add the deploy.yaml file with the configuration provided above.
2- Configure GitHub Pages:
  - Go to your repository settings.
  - Enable GitHub Pages and set the source to the gh-pages branch.
  - Ensure that GitHub Actions have the required read/write permissions.
3- Modify vite.config.js for GitHub Pages:
  - If your project is deployed to a subdirectory, update your vite.config.js file with the correct base path:
```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'

export default defineConfig({
  plugins: [react()],
  base:'/ci-cd-Pipeline/'
})
```
### Troubleshooting and Bug Fixes
1- Permissions Issue:
  - Problem: GitHub Actions did not have permission to push to the repository.
  - Solution: Updated the repository settings to give GitHub Actions the necessary read/write permissions.
2- Image Loading Issue:
  - Problem: After deployment, images were not loading because the project was not correctly configured to handle the base path.
  - Solution: Updated `vite.config.js` to include the correct base path.

### Tools and Practices
#### Tools Used
- GitHub Actions: Automates the CI/CD pipeline, reducing manual intervention.
- peaceiris/actions-gh-pages: Simplifies the deployment process to GitHub Pages.
- Vite: Used as the build tool for the React application.
- Node.js: The runtime environment used to build the project.
#### Benefits
- Automation: Ensures consistency in the deployment process, reducing the potential for human error.
- Continuous Integration: Automatically builds and tests the project, catching issues early in the development process.
- Continuous Deployment: Deploys the latest changes to GitHub Pages automatically, allowing for rapid feedback and iteration.

### Missing Elements and Future Enhancements
#### Automated Testing
While the current pipeline focuses on building and deploying the project, integrating automated testing is a recommended next step to ensure code quality. You can add a testing step in the pipeline:
```
- name: Run Tests
  run: npm test
```

### Infrastructure as Code (IaC)
While this project is focused on deploying a static site, implementing Infrastructure as Code using tools like Terraform or Ansible could be beneficial for more complex projects. This would ensure that the environment is consistently configured across different deployments.

### Conclusion
This CI/CD pipeline setup ensures that the React Vite project is built, tested, and deployed automatically with every push to the `master` branch. By leveraging GitHub Actions and GitHub Pages, the deployment process is streamlined, allowing for rapid and reliable delivery of updates.

If you have any questions or need further assistance, feel free to reach out or submit an issue on this repository.

This README covers all the necessary details of your project, ensuring that all the deliverables and requirements are addressed. You can place this in your repository as `README.md`.





