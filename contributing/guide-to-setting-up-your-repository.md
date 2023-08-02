
# Guide to Setting Up Your Repository - Get Started Easily!

This guide will help you to setup your repository initially :smile:

## Create a new repository and set a license

NOTE - In our organization, this section could be SKIPPED becuase the organization owner would create and share repositories.

1. Access an GitHub organization

(An example of Github organization)
<p align="center">
  <img src="https://github.com/cloud-barista/docs/assets/7975459/66801023-6a8a-4f88-b770-9856da73790a" width="90%" height="90%" >
</p>

2. Click "New" to create a repository
  - Set `Repository name`
  - Select `Public`
  - Check `Add a README file`
  - Choose `Go` for `.gitignore` template <ins>(set this appropriately for your condition)</ins>
  - Choose `Apache License 2.0` for LICENSE

(An example of "Create a new repository")
<p align="center">
  <img src="https://github.com/cloud-barista/docs/assets/7975459/181e02aa-5434-4a11-9d0a-35763e7aeeac" width="70%" height="70%" >
</p>


## Add CODE-OF-CONDUCT.md

1. See examples below:
  - [CB-Spider's CODE-OF-CONDUCT.md](https://github.com/cloud-barista/cb-spider/blob/master/CODE_OF_CONDUCT.md)
  - [CB-Tumblebug's CODE-OF-CONDUCT.md](https://github.com/cloud-barista/cb-tumblebug/blob/main/CODE_OF_CONDUCT.md)

2. Add code of conduct to the repository


## Add CONTRIBUTING.md

1. See examples below:
  - [CB-Spider's CONTRIBUTING.md](https://github.com/cloud-barista/cb-spider/blob/master/CONTRIBUTING.md)
  - [CB-Tumblebug's CONTRIBUTING.md](https://github.com/cloud-barista/cb-tumblebug/blob/main/CONTRIBUTING.md)

2. Add CONTRIBUTING.md to the repository


## Update README.md

NOTE - Create README.md if it does not exist in the repository

1. See examples below:  
  - [CB-Spider's README.md](https://github.com/cloud-barista/cb-spider/blob/master/README.md)
  - [CB-Tumblebug's README.md](https://github.com/cloud-barista/cb-tumblebug/blob/main/README.md)

2. Update README.md
  NOTE - For the initial version, it would be nice to have an overview of the repository. 
  The contents of README will continue to improve.


## Create teams
We will create a parent team and 2 child teams as follows.

(See [About teams](https://docs.github.com/en/organizations/organizing-members-into-teams/about-teams))

  <p align="center">
  <img src="https://github.com/cloud-barista/docs/assets/7975459/516a9f16-367c-4750-90b1-d2884cf0a3b2" width="90%" height="90%" >
  </p>

1. Access `teams` tab on the organization 
  <p align="center">
  <img src="https://github.com/cloud-barista/docs/assets/7975459/724d1c7f-7833-46a2-a173-75214d2acf71" width="90%" height="90%" >
  </p>

2. Click "New team" to create a parent team
  - Set `Team name` (In this example, `cb-larva` is used.)
  - Set `Description`
  - **SKIP `Parent team`**
  - Select `Visible` for team visibility
  - Select `Enabled` for team notifications

  (Example - "Create new team")
  <p align="center">
  <img src="https://github.com/cloud-barista/docs/assets/7975459/bf36ec4c-105e-4471-8c27-87b6dcd4a656" width="50%" height="50%" >
  </p>

3. Click "New team" to create child teams
  - Set `Team name` (In this example, `cb-larva-maintainers` and `cb-larva-contributors` are used.)
  - Set `Description`
  - **Select `Parent team` created in the previous section** (e.g., `cb-larva`)
  - Select `Visible` for team visibility
  - Select `Enabled` for team notifications


## Set permissions of repository members

NOTE - See [Repository roles for an organization](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/repository-roles-for-an-organization)

1. Access `Repository` > `Settings` > `Collaborators and teams`
2. Click "Add teams" to set permission for **maintainers**
  - Search and select a team (e.g., `cb-larva-maintainers`)
  - Choose `Maintain` role

3. Click "Add teams" to set permission for **contributors**
  - Search and select a team (e.g., `cb-larva-contributors`)
  - Choose `Triage` role

(Example - "Add teams")
  <p align="center">
  <img src="https://github.com/cloud-barista/docs/assets/7975459/85c8c670-9c5b-402f-90ec-b1efa6345e63" width="60%" height="60%" >
  </p>

