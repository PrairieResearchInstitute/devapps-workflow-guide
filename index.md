# DevApps Workflow Guide

This guide describes how the PRI DevApps team develops and maintains applications using Git, GitHub, TDX and Visual Studio.

The goal is simple: make changes traceable, reduce risk, and make it easier for more than one person to understand and support the code over time.

---

## Why this matters

Right now, much of the development work happens directly on deployed machines. That works until something breaks. When it does, it is hard to know what changed, when it changed, or how to undo it. It is also difficult for someone else to step in and help.

A basic Git-based workflow solves these problems in a practical way. It gives us history, safer changes, a place for review, and a clear connection between a request and the code that implements it.

There is one idea that underpins everything in this guide:

> Production is not the development environment.

---

## How we work

Every change starts with a TDX ticket. The ticket is where the context lives. It should describe the problem or request clearly, and it should be obvious who owns the work.

From there, the developer creates a branch from `main` that is tied to that ticket. The branch name should make that connection obvious. For example:

```bash
tdx-18452-fix-mileage-rounding
```

Work happens in that branch, in a local working copy of the code. Changes are committed as the work progresses, using short and clear messages that include the TDX number.

Once the change is ready, the branch is pushed to GitHub and a pull request is opened into `main`. The pull request is where the developer explains what changed, why it changed, and how it was tested.

Another team member should review the change when possible. This does not need to be heavy process. The point is simply to have a second set of eyes before the change becomes part of the main codebase.

After that, the change is merged into `main`. For this team, using squash merge is a good default because it keeps the history clean and easy to read.

Deployment should always come from the repository, not from a developer’s working folder.

---

## The rules we follow

There are a few rules that make this workflow work.

Every meaningful change must have a TDX ticket. Every ticket gets its own branch. No one pushes directly to `main`. Changes are merged through pull requests. Production code is not edited directly, except in emergencies, and even then the fix must be brought back into Git immediately.

If we follow just these rules, the workflow already becomes much safer and easier to manage.

---

## Day-to-day workflow

A typical piece of work looks like this.

Start by pulling the latest version of `main`. Then create a new branch for your ticket.

```bash
git switch main
git pull origin main
git switch -c tdx-18452-fix-mileage-rounding
```

Make your changes locally and commit them as you go.

```bash
git add .
git commit -m "TDX 18452: fix mileage reimbursement rounding"
```

When the work is ready, push the branch.

```bash
git push -u origin tdx-18452-fix-mileage-rounding
```

Open a pull request in GitHub and describe what you did and how you tested it. Once it is reviewed, merge it into `main`.

From there, deploy using the code from the repository.

---

## Getting set up on Windows

Each development machine needs Git installed. This can be a local workstation or a remote Windows server.

After installing Git, verify it:

```bash
git --version
```

Each developer should configure their identity once:

```bash
git config --global user.name "Full Name"
git config --global user.email "name@illinois.edu"
```

It also helps to set a default branch:

```bash
git config --global init.defaultBranch main
```

Each developer should have a GitHub account and access to the `PrairieResearchInstitute` organization.

---

## Using Git with Visual Studio

Visual Studio already supports Git, so most of the workflow can happen inside the IDE.

From Visual Studio, developers can:

* clone a repository
* create and switch branches
* commit changes
* push and pull
* open pull requests

There is no need to use the command line unless you want to. The important thing is that the workflow is followed, not how the buttons are clicked.

---

## Bringing existing code into GitHub

Several PRI applications currently exist only as folders on a machine. The right way to move them into GitHub is to start from a clean working copy, not from the deployed application directory.

Create a separate working folder, for example:

```text
C:\Dev\active-grants
```

Copy the code there, then remove anything that should not be version controlled. This includes logs, compiled output, machine-specific files, and any secrets.

Then initialize Git:

```bash
cd C:\Dev\active-grants
git init
git branch -M main
git add .
git commit -m "Initial import of existing Active Grants application"
```

Create a new repository in GitHub and connect it:

```bash
git remote add origin https://github.com/PrairieResearchInstitute/active-grants.git
git push -u origin main
```

After pushing, check GitHub to make sure only the expected files are present.

---

## Conventions

We keep conventions simple and consistent.

Branch names follow this pattern:

```text
tdx-<ticketnumber>-short-description
```

Commit messages should include the TDX number:

```text
TDX 18452: fix mileage reimbursement rounding
```

Pull requests should explain what changed, why it changed, and how it was tested.

Each application should have its own repository unless there is a clear reason not to.

---

## Handling emergencies

Sometimes something breaks in production and needs to be fixed quickly. In those cases, restoring service is the priority.

Even then, the fix must be captured properly. Create or update a TDX ticket, make the smallest safe change, and bring that change back into Git through a branch and pull request as soon as possible.

The repository must remain the source of truth.

---

## Final notes

This workflow is not about adding process for its own sake. It is about making everyday work safer and easier to understand.

For a small team, this level of structure goes a long way. It makes onboarding easier, reduces risk, and helps ensure that knowledge does not live in just one person’s head.
