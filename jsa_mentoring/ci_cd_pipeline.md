# CI pipeline

## What is CI ?

---

> [[Tutorial] Continuous Integration with CircleCI and NodeJS](https://medium.com/meshstudio/continuous-integration-with-circleci-and-nodejs-44c3cf0074a0)
> 
> CI is the process of building and testing a code base whenever an individual developer pushes their contributions to a repository. 
> 
> This process needs to be automated, meaning a system is responsible for building and testing the code base, not a human.

## Getting Started

--- 

Our CI build process is going to consist of the following two steps:

1. Install dependencies ``npm install``
2. Execute the test suite ``npm test``

## Setting up CircleCI

---

Our CI provider of choice at Mesh is **CircleCI**. To run builds on Circle, we need to create a new directory in the root of our project called ``.circleci``.

```
$ mkdir .circleci
$ touch .circleci/circle.yml
```

The **circle.yml** file will contain the build instructions that CircleCI follows for your build.

Copy and paste the following code into your **circle.yml** file.
```
version: 2
jobs:
  build:
    docker:
      — image: circleci/node:7.10
    working_directory: ~/repo
    steps:
      — checkout
      - run: npm install
 
      — run: npm test
```

## The circle.yml File

---

- ### **Jobs**

The jobs section of the **circle.yml** defines a **task** or set of tasks to be run for the CI process.

- ### **Build**

The build section defines the **name for the job**.

- ### **Docker**

The docker section defines an **executor for the build process**.

> **An executor is a place where build steps will occur.**

By specifying docker, we are telling Circle that we want our build steps to take place inside of a Docker container.

- ### **Image**

When we use the docker executor, we must define the **image** that we want to use for the executor.

In this case, we have decided to use a Circle image that supports **node.js v7.10.**

## **Working Directory**

- ### **Steps**

The steps section defines a list of **steps** that need to be executed to complete the build process.

Each of these steps must pass without failure; otherwise, the CI process will fail.

We describe the steps, and their functionality below:
>
> ``checkout`` — A CircleCI built in command which is responsible for ***checking out the project’s source code into the Job’s working_directory*** we discussed above.
>
> ``run: npm install`` — Installs all project dependencies listed in the **package.json** file.
>
> ``run: npm test`` — Executes the **automated test suite**

## Running the build locally

---

