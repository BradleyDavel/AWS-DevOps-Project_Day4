<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

<p align="center">
  <a href="https://aws.amazon.com/codebuild/"><img src="https://img.shields.io/badge/AWS-CodeBuild-orange?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS CodeBuild"></a>
  <a href="https://aws.amazon.com/codeartifact/"><img src="https://img.shields.io/badge/Integration-CodeArtifact-blueviolet?style=for-the-badge&logo=artifacthub&logoColor=white" alt="AWS CodeArtifact Integration"></a>
  <a href="https://github.com/BradleyDavel"><img src="https://img.shields.io/badge/Maintainer-Bradley%20Davel-success?style=for-the-badge&logo=github" alt="Maintainer"></a>
  <a href="http://learn.nextwork.org/projects/aws-devops-codebuild-updated"><img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge&logo=checkmarx" alt="Project Status"></a>
  <a href="https://choosealicense.com/licenses/mit/"><img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge&logo=open-source-initiative&logoColor=white" alt="License"></a>
</p>


# Continuous Integration with CodeBuild

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codebuild-updated)

**Author:** Bradley Davel  
**Email:** bradleydavel123@gmail.com

---

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-codebuild-updated_35588a47)

---

## Introducing Today's Project!

In this project, I'll be setting up a continuous integration system in CodeBuild! We will do the following in this project:

🛠️ Create and configure a CodeBuild project from scratch.
🔗 Connect your CodeBuild project to your GitHub repository.
⚙️ Define your build process using a buildspec.yml file.
💎 Automate testing using CodeBuild too!

### Key tools and concepts

✅ Key Services and Concepts I Learnt in This Project:

⚙️ AWS CodeBuild – I learned how CodeBuild works as a managed Continuous Integration (CI) service to automate compiling, testing, and packaging applications without maintaining my own build servers.

📦 AWS CodeArtifact – I practiced setting up a private package repository, pulling dependencies securely, and publishing my own build artifacts.

🛡️ IAM Roles & Policies – I gained hands-on experience in applying the principle of least privilege by configuring CodeBuild’s IAM role with the exact permissions needed to access CodeArtifact and S3.

💾 Amazon S3 – I learned how S3 can be used to store build artifacts (like .war or .jar files) so they can be deployed later in the pipeline.

📊 CloudWatch Logs – I saw how enabling logs provides visibility into the build process, making it easier to monitor, troubleshoot, and audit build runs.

📄 buildspec.yml – I understood how to define build phases (install, pre_build, build, post_build)

### Project reflection

⏱️ Duration:
This project took me approximately 45 minutes to complete.

⚡ Challenge:
The most challenging part was correcting unnecessary errors during the build process, which taught me the importance of carefully reviewing configurations and permissions.

🏆 Rewarding Part:
It was most rewarding to see the build succeed end-to-end after troubleshooting, with all services (CodeBuild, CodeArtifact, IAM, S3, and CloudWatch) working together as intended. That confirmed my understanding of how a secure and automated CI pipeline operates in AWS.

This project is part four of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project later today as well.

---

## Setting up a CodeBuild Project

CodeBuild is a continuous integration (CI) service, which means it automatically compiles, tests, and packages code every time developers make changes. Instead of manually running builds on local machines, a CI service provides a consistent, automated environment where code changes are integrated frequently and tested early.

Engineering teams use it because it:

-Catches bugs early by running tests as soon as new code is pushed.

-Saves time and resources by automating repetitive build tasks.

-Improves collaboration since everyone’s code is validated against the same shared build pipeline.

-Scales easily without the need to manage dedicated build servers.

✅ In short: A CI service like CodeBuild ensures that code changes are continuously integrated, tested, and packaged in a reliable, automated way — which is a cornerstone of modern DevOps practices.

CodeBuild doesn't know where your web app's code lives! Source means the location of the code that CodeBuild will fetch, compile, and package into a file you can deploy.

We're choosing GitHub as our source provider because that's where we've stored our web app's code. By connecting CodeBuild directly to GitHub, we're creating a seamless pipeline where code changes automatically flow into our build process.

CodeBuild plays well with lots of different code repositories - AWS CodeCommit, Bitbucket, GitHub, and even plain S3 buckets. This flexibility lets you keep using whichever code storage system your team prefers while still getting all the benefits of AWS's build capabilities

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-codebuild-updated_fewgrhte)

---

## Connecting CodeBuild with GitHub

This is generally the simplest and most secure option. AWS manages the application and connection, reducing the need for you to handle tokens or keys directly. It's recommended for most use cases due to its ease of use and enhanced security.

AWS CodeConnections is like a secure bridge between AWS and your external code repositories. Instead of dealing with the headache of managing API keys, tokens (like GitHub's Personal Access Tokens!), or SSH credentials, CodeConnections handles all that authentication complexity for you - so you can focus on building your application.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-codebuild-updated_a7c98e2d)

---

## CodeBuild Configurations

### Environment

I set up my CodeBuild project’s environment by:

Selecting Amazon Linux as the operating system to provide a secure, stable build environment.

Choosing the Standard runtime with the Corretto 8 image to support my Java-based project.

Creating a new IAM service role specifically for CodeBuild, ensuring it had the least-privilege permissions needed to pull code.

This configuration ensured that the build ran in a clean, consistent environment with proper security and dependency support.

### Artifacts

The key artifact that this S3 bucket will store is the output of the CodeBuild process — typically the packaged web application (such as a .jar, .war, or .zip file) generated after compiling and building the project. This artifact represents the deployable version of the application that can later be used by other services in the CI/CD pipeline, such as CodeDeploy or Elastic Beanstalk.

### Packaging

Packaging artifacts as a Zip file is a small detail that's actually quite useful:

Smaller size: Zip compression can significantly reduce the size of your artifacts, which means faster uploads to S3, less storage costs, and quicker downloads when you need to use them.

Organization: Instead of managing multiple individual files, you get one tidy package. It's like putting all your vacation photos in a single album rather than having them scattered across your phone.

Simplicity: When it comes time to deploy your application or share it with teammates, having a single zip file makes the process much more straightforward - just download one file and you have everything you need.

### Monitoring

We are using CloudWatch Logs to capture and monitor the output of our CodeBuild project in real time. This is important because:

📖 Visibility – It provides a detailed record of each build step, making it easier to understand what happened during the build.

🐞 Troubleshooting – If a build fails, the logs show the exact error messages and context, which helps quickly identify and fix issues.

📊 Monitoring & Metrics – Logs can be turned into CloudWatch metrics and alarms, so we can track build performance, detect failures, and respond proactively.

🛡️ Auditing – It creates a permanent record of build executions for compliance and accountability.

✅ In short: CloudWatch Logs ensure that every build is observable, debuggable, and auditable, which is essential for reliable CI/CD pipelines.

---

## buildspec.yml

My first build failed because we haven't created the .yaml file needed for the project yet. A buildspec.yml file is needed because CodeBuild reads your buildspec.yml file like a step-by-step instruction manual. It goes through each phase in order - install, pre_build, build, then post_build - running the commands you've specified in each section.

The beauty of using buildspec.yml is that your build process is defined as code right alongside your application. This means your build process can be versioned, reviewed, and evolved just like any other part of your codebase. Before the era of continuous integration (CI), you would have to manually run a bunch of commands to build your application - buildspec.yml automates this process!

Install Phase (prep work): In this phase, CodeBuild prepares the build environment. For my project, I set it to use Java 8 (Corretto) so the right runtime is available.

Pre_build Phase: These are the tasks that must run before the main build. In my case, I grab a CodeArtifact security token so the build process can authenticate and pull dependencies securely.

Build Phase: This is the core of the process, where the application is actually compiled. I’m using Maven, a widely used Java build tool, to compile the code into bytecode.

Post_build Phase: This phase handles the finishing steps after the build succeeds. For my project, I package the compiled code into a WAR file, which is the standard format for deploying Java web applications.

✅ In short: The buildspec.yml phases move step-by-step from setting up the environment → authenticating → compiling → packaging into a deployable artifact.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-codebuild-updated_35588a47)

---

## Success!

My second build also failed, but with a different error that said the following:

-COMMAND_EXECUTION_ERROR: Error while executing command: mvn -s settings.xml compile. Reason: exit status 1
-COMMAND_EXECUTION_ERROR: Error while executing command: mvn -s settings.xml package. Reason: exit status 1
-CLIENT_ERROR: no matching artifact paths found


To resolve the second error, I granted the CodeBuild IAM service role the required permissions to access CodeArtifact (e.g., codeartifact:GetAuthorizationToken, codeartifact:GetRepositoryEndpoint, and codeartifact:ReadFromRepository). After updating the role and rebuilding, the build completed successfully.

By confirming that an artifact exists in your S3 bucket, you know that:

-Your code was successfully compiled and packaged
-The build process completed without errors
-The artifact was properly uploaded to its destination
-The artifact is now available for the next step in your process (like deployment)

Think of it as the final proof that the CI process is delivering what it promised!

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-codebuild-updated_d9cc6191)

---

