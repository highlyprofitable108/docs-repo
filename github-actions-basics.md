## Updated GitHub Actions Basics Guide

### Introduction
Welcome to the updated GitHub Actions Basics Guide. This guide is designed to provide you with a comprehensive understanding of GitHub Actions, an essential tool for automating your software development process directly within your GitHub repository. From automating builds, tests, to deployments, GitHub Actions can significantly boost productivity and ensure a consistent workflow.

### Core Concepts

#### GitHub Actions Overview
GitHub Actions is a CI/CD (Continuous Integration and Continuous Delivery) service that allows you to automate your software development workflows. With workflows defined in YAML files, you can automate processes based on GitHub events such as push, pull requests, issue creation, or even on a schedule.

#### Workflows
Workflows are custom automated processes that you define in your repository. Stored in the `.github/workflows` directory, these YAML files describe the sequence of actions to be performed when triggered by specific GitHub events or at defined times.

#### Actions
Actions are the building blocks of workflows. They can be individual tasks or a set of operations that can run commands in a workflow. You can create custom actions or utilize shared community actions for a wide array of automation purposes.

#### Runners
Runners are the servers where workflows are executed. GitHub provides hosted runners with various operating systems, or you can opt for self-hosted runners for more specific control over the running environment.

#### Triggers
Workflows can be triggered by various GitHub events, such as pushes to a branch, pull request creations, or even on a schedule using cron syntax. Manual triggers through the GitHub UI are also supported, offering flexibility in workflow execution.

#### Environment Secrets
Sensitive information required by a workflow, such as API keys or credentials, should be stored as encrypted secrets in the repository settings. These secrets can then be safely referenced in workflows, maintaining security.

#### Matrix Builds
The matrix strategy in workflows allows for the parallel execution of jobs across different environments. By specifying a matrix of different operating systems and language versions, you can efficiently test your application across multiple scenarios.

#### Scheduled Workflows
Workflows can be configured to run on a schedule using cron syntax, enabling automated tasks like nightly builds or routine tests to run without manual intervention.

### Getting Started

1. **Workflow Syntax**: Start by understanding the YAML syntax for workflows, including defining triggers, jobs, steps, and actions.

2. **First Workflow**:
   - Go to your repository on GitHub.
   - Create a `.github/workflows` directory if it's not already present.
   - Add a new YAML file to define your workflow, such as `ci.yml`.

3. **Workflow Definition**:
   - Define triggers for when the workflow should run, like on push or pull request events.
   - Organize the workflow into jobs, which can contain multiple steps. These steps can execute actions, run scripts, or perform tasks.
   - Leverage actions within steps to accomplish specific tasks, such as setting up environments or deploying code.

4. **Launch and Monitor**:
   - After committing your workflow file, watch it in action under the Actions tab in your GitHub repository to ensure it performs as expected.

### Practical Implementation Tips

- **Use Matrix Builds** to ensure your application behaves correctly across different systems and versions.
- **Implement Caching** to speed up builds by reusing stored data between workflow runs.
- **Securely Manage Secrets** for any sensitive data needed by your workflows.
- **Take Advantage of Scheduled Runs** for maintenance tasks or routine checks.

### Composite Actions

Composite actions allow you to combine multiple steps into a single action. Unlike individual actions that perform a singular task, composite actions bundle a set of commands, shell scripts, and other actions into one reusable action. This is particularly useful for encapsulating a sequence of steps that you find repeating across multiple workflows.

#### Location and Usage

Unlike workflow files, composite actions are defined in a dedicated repository and not in the .github/workflows directory. To use a composite action, you reference it in your workflow file with its repository name, much like you would with any other action.

### Reusable Actions

Reusable actions take the concept of DRY (Don't Repeat Yourself) to the next level in GitHub Actions. By allowing workflows to call other workflows, reusable actions enable you to share automation sequences across projects and repositories, reducing duplication and fostering consistency.

#### Benefits
Reusable actions simplify workflow management by centralizing common processes. They can be updated in a single location, with changes propagating to all workflows that utilize them.

#### Location and Usage

Defined in their own repository, reusable workflows are referenced in your workflow files using the uses keyword with the repository's URL. This approach differs from composite actions in that entire workflows, complete with jobs and steps, are reused.

### Getting Started with Composite and Reusable Actions

#### Creating Composite Actions:

In a repository, create a new directory to house your action, e.g., .github/actions/<action_name>.
Within this directory, add a action.yml file to define the action inputs, outputs, and the series of runs or commands that make up the action.

Use the uses field in your workflow file to reference the composite action, specifying the path to the action.yml file.

#### Implementing Reusable Workflows

Define a standalone workflow in a repository, which includes jobs, steps, and actions as you would in any other workflow.

In the calling workflow, use the uses keyword followed by the repository name and the path to the workflow file, along with any needed inputs.

Reusable workflows can be invoked from the same repository or from a different one, making them highly versatile for cross-project automation.

### Conclusion

GitHub Actions stands as a pivotal tool for automating and optimizing your software development workflows within the GitHub ecosystem. Through a deep understanding of workflows, actions, runners, and the strategic use of environment secrets and matrix builds, you can craft highly efficient and secure CI/CD pipelines. As you explore the vast possibilities offered by GitHub Actions, remember that experimentation and adaptation are key to unlocking the full potential of automated workflows tailored to your project's specific needs.

### Exercise: Building and Executing a Basic Ad-Hoc Action with Java

#### Scenario
Your project is transitioning to newer versions of Java for enhanced features and security improvements. Currently, the project supports Java 17, but there's an initiative to also ensure compatibility with Java 21. This requires modifications and testing across different Java versions to ensure the application remains stable and performs as expected.

#### Objective
Create a GitHub Action to build and test the project using both Java 17 and Java 21. This workflow should automate the process whenever changes are pushed to the `main` branch or when a pull request targeting `main` is opened.

### Steps to Create and Execute the Workflow

1. **Prepare Your Workflow File**:
   - Navigate to your project's repository on GitHub.
   - Create a new file under the `.github/workflows` directory. You might name it `java-ci.yml` to reflect its purpose.

2. **Define the Workflow**:
   - Start by specifying the name of the workflow, for instance, `Java CI`.
   - Define the trigger events: pushes and pull requests to the `main` branch.
   - Set up a job that includes a strategy for matrix builds across the two Java versions (17 and 21).
   - Include steps to check out the code, set up the JDK for the matrix's Java versions, build the project, and run tests.

3. **Workflow Code Sample**:
   ```yaml
   name: Java CI

   on:
     push:
       branches: [ main ]
     pull_request:
       branches: [ main ]

   jobs:
     build:
       runs-on: ubuntu-latest

       strategy:
         matrix:
           java-version: [17, 21]

       steps:
       - uses: actions/checkout@v3
       - name: Set up JDK ${{ matrix.java-version }}
         uses: actions/setup-java@v3
         with:
           java-version: ${{ matrix.java-version }}
           distribution: 'temurin' # AdoptOpenJDK, now Eclipse Temurin
       - name: Build with Maven
         run: mvn -B package --file pom.xml
       - name: Run Tests
         run: mvn test
   ```

4. **Commit and Push the Workflow File**:
   - Save your changes and commit the `java-ci.yml` file to your repository. Push the commit to GitHub.

5. **Observe the Action**:
   - Once committed, navigate to the Actions tab of your GitHub repository to observe the workflow execution. It should trigger automatically based on the events you defined (push or pull request to `main`).

6. **Review Results**:
   - After the workflow completes, review the job logs to ensure the build and tests pass for both Java versions. Address any failures as necessary by iterating on your code and pushing changes.

### Conclusion
This exercise demonstrates the power of GitHub Actions for automating CI/CD processes, specifically here for ensuring compatibility across different Java versions. By leveraging matrix builds, you can efficiently test your application against multiple environments, significantly improving the reliability and stability of your software.

## Introduction to GitHub Actions Quiz

This quiz is designed to test your foundational understanding of GitHub Actions, focusing on basic concepts, workflow creation, and automation practices. For each question, select the best answer based on the information provided.

### Question 1: Workflow Triggers

Which event cannot be used to trigger a GitHub Actions workflow?

- A) A push to a specific branch
- B) A pull request creation
- C) A scheduled time using cron syntax
- D) A manual trigger from the GitHub UI
- E) A commit is directly pushed to an external repository not linked via submodules

### Question 2: Action Execution Environment

Where do GitHub Actions run?

- A) Only on GitHub-hosted servers
- B) On the developer's local machine automatically
- C) On GitHub-hosted runners or self-hosted runners
- D) Directly within the GitHub repository without any execution environment

### Question 3: Secrets Management

How should you securely use sensitive data like passwords or API keys in GitHub Actions?

- A) Hard-code them directly into the workflow YAML file
- B) Use GitHub Secrets and reference them in your workflows
- C) Store them in a public repository for easy access
- D) Email them to all developers to manually input when needed

### Question 4: Reusable Workflows

What is a reusable workflow in GitHub Actions?

- A) A workflow that can only be run once and needs to be recreated for each use
- B) A workflow that can be called from another workflow within the same repository or from a different repository
- C) A template that GitHub provides for all common programming languages and cannot be modified
- D) A workflow that automatically duplicates itself every time it runs

### Question 5: Matrix Strategy

What is the purpose of a matrix strategy in GitHub Actions workflows?

- A) To limit the workflow to run only on the first job in the matrix to save time
- B) To simultaneously run jobs across different combinations of environments, such as operating systems and programming language versions
- C) To sequentially run jobs to ensure that each one starts only after the previous one has completed successfully
- D) To randomly select an environment to run tests in, to save on computing resources

---

### Answer Key

**1. Correct Answer: E**  
**Explanation:** GitHub Actions cannot be triggered by direct pushes to external repositories not linked via submodules. Triggers like push, pull request, cron schedules, and manual triggers via the UI are supported.

**2. Correct Answer: C**  
**Explanation:** GitHub Actions can run on GitHub-hosted runners, which offer various operating systems, or on self-hosted runners, which you can set up in your own environment for more control.

**3. Correct Answer: B**  
**Explanation:** Using GitHub Secrets is the secure way to manage sensitive data in GitHub Actions. Secrets are encrypted and allow you to reference sensitive information without exposing it in your workflow files.

**4. Correct Answer: B**  
**Explanation:** A reusable workflow allows you to call a workflow from another workflow, enabling code reuse across different workflows within the same or different repositories. This helps maintain consistency and reduce duplication.

**5. Correct Answer: B**  
**Explanation:** The matrix strategy in GitHub Actions enables you to run jobs across different environments by creating a job for each combination of the provided variables, such as operating system versions and programming language versions. This approach is used for comprehensive testing across multiple environments.
