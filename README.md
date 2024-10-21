
# The School of Commonwealths (SoC)

The innovative model of the education system in which traditional classroom-based activities are
transformed into globally connected communication systems (Pierzchalski, 2022). These systems are
created by linking academic classes with similar topics between any universities worldwide. The
linking is facilitated using GitHub. In the proposed model, students still study in the halls of the
universities but additionally use the GitHub service to communicate with each other, involving the
review of assignments/tasks analogous to the work of scientists who review each other's
publications. In this way, students develop critical thinking and foster a culture of
collaboration. For the new school to function, there is a need for materials that could be utilized
by lecturers from the combined universities. This repository contains materials and instructions to
start the School of Commonwealths.

# Prerequisites

1. **Find a partner** from any university worldwide you prefer to combine data science classroom-based
   activities into one communication system.
2. **Create a GitHub account.**
   - If you don’t already have one, create a GitHub account: https://github.com/
3. **Create an organization on GitHub.**
   - With your GitHub account, create an organization with a Free plan. On your account, choose:
     Settings ➡ Organizations ➡ New orgranization; or click the link: [create your
     organization](https://github.com/account/organizations/new?plan=team_free)
   - Upgrade your organization by clicking the '[Upgrade to GitHub
     Team](https://education.github.com/globalcampus/teacher)' button and selecting the organization
     you want to upgrade. It is good idea to create as many organizations as the number of courses
     you run. (Note: At least one organization owner is required to have a Pro account and must be
     verified as a teacher to upgrade an organization for free. You can get a discount for a Pro
     account by verifying your teacher status here:
     [discount_requests_application](https://education.github.com/discount_requests/application))
   - Add your partner to your organization as an 'Owner', ensuring that both of you have equal
     rights and full administrative access to the entire organization (in order for a partner to
     fully utilize the course management software, they must be assigned the 'Owner' role).
4. **Install & set up Git.**
   - download and install Git:
     - Linux, Mac: https://www.git-scm.com/downloads
     - Windows: https://gitforwindows.org/
   - configure your name and email, which will be used to identify the author of commits:

   ```shell
   git config --global user.name "Your Name"
   git config --global user.email "your_email@example.com" # this is optional
   ```

5. **Install & set up GitHub CLI**
   - download and install GitHub CLI (gh): https://cli.github.com/
   - log in to GitHub and grant gh additional permissions. When prompted for
    your preferred protocol for Git operations, select HTTPS, and when asked if
    you would like to authenticate to Git with your GitHub credentials, enter
    Y. With that, the GitHub CLI will automatically store your Git credentials
    for you, so that you don't have to enter your credentials for every
    operation requiring authorization.

   ```shell
   gh auth login --scopes admin:org,delete_repo,project
   ```

6. **Install [the extension to manage the SoC](https://github.com/pierzcham/gh-soc)**:

   ```shell
   gh extension install pierzcham/gh-soc
   ```

7. **Install GitHub App 'Czujnikownia'.**
   - With your GitHub account, install https://github.com/apps/czujnikownia. This app is used to
   record the times of submitting assignments and reviews to which students have been assigned.

# Let's start

1. **Initialize the SoC Data Science Course locally in a folder of your choice.**

   ```shell
   gh soc init
   ```

   The `init` command creates configuration files `user_config.sh`, `source_config.sh`,
   `system_config.sh`, and an additional file for student emails named `emails.txt`. It also creates
   a folder named 'logs' to store JSON data files.

   - Adjust the settings in the `user_config.sh` file, which already contains default values, to
   your preferences.

   ```shell
   GH_ORG_NAME='' # insert here your GitHub organization name
   GH_TEAM_NAME='SOC-DS'
   GH_TEAM_DESCRIPTION='SoC Data Science Course'
   GH_TEAM_MEMBERS_EXCLUDED_FROM_REVIEWING=()
   TARGET_REPO_DESCRIPTION='SoC Data Science Course'
   TARGET_REPOS=(...) # for default values, look in the file
   TARGET_PROJECT_TITLE='Monitor-SoC-DS'
   ```

   - Add your students' emails to `emails.txt`. This file will be used by `open` command to send
   invitations to join your GitHub organization 'GH\_ORG\_NAME' and automatically add them to the
   team 'GH\_TEAM\_NAME'.

   Any time you can run a precheck before each use of other commands to make sure your environment
   is ready to run.

   ```shell
   gh soc precheck
   ```

2. **Synchronize the SoC Data Science Course with the source repositories.**

   ```shell
   gh soc sync
   ```

   This command creates a [GitHub
   project](https://docs.github.com/en/issues/planning-and-tracking-with-projects) titled
   'TARGET\_PROJECT\_TITLE' from the 'SOURCE\_PROJECT\_OWNER' GitHub account, as specified in
   `source_config.sh`. This project is used to gather information on work progress. Subsequently,
   repositories with names defined in 'TARGET\_REPOS' array in the `user_config.sh` file are created
   based on the template repostories defined in 'SOURCE\_REPOS' array in the `source_config.sh`
   file. If the source repositories are not already templates, it will attempt to convert them. This
   is useful if you want to use your own repositories as a source and automatically convert them to
   templates.

   At any time, you can unsynchronize your course from the source repositories by deleting project
   and all the repositories forked from that source .

   ```shell
   gh soc unsync
   ```

3. **Open the SoC Data Science Course**

   ```shell
   gh soc open
   ```

   Opening a course involves, in addition to making some necessary organizational settings, creating
   a 'GH_TEAM_NAME' team to which students will be added after they accept the invitations to join
   the organization sent by the `invite` command. Team settings are adjusted here to ensure the
   mechanism for the automatic selection of a reviewer functions properly. Individuals excluded from
   reviewing, such as teachers, are listed in 'GH\_TEAM\_MEMBERS\_EXCLUDED\_FROM\_REVIEWING'. At
   this stage, task repositories are cloned to a local folder. Next, repositories containing tasks
   are configured to protect them against modifications by students – the main branch is
   locked. Students are required to fork the repository to their account in order to make their
   changes. Finally, the GitHub Pages for the course is enabled and accessible from the web.

   At any time you can close your course.

   ```shell
   gh soc close
   ```

   The `close` command concludes a course by archiving project data and optionally deleting the
   team associated with the course. When deleting a team, you can optionally remove team members
   from your organization.

4. **Invite students to join the course**


   Sends invitations to students using the email addresses listed in the `emails.txt` file

   ```shell
   gh soc invite
   ```

   The command below deletes the invitations for students based on the email
   addresses found in the

   `emails.txt` file.

   ```shell
   gh soc disinvite
   ```

5. **Run your SoC Data Science Course by assigning tasks to your team one at a time.**

   ```shell
   gh soc assign <repository name> # e.g. gh soc assign soc-datascience-hello
   ```

   At any time, you can unassign the repository from your team.

   ```shell
   gh soc unassign <repository name>
   ```

   Moreover, you can save the current state of your team's progress.

   ```shell
   gh soc log
   ```

   The `log` commnad captures and logs the current state of a specified GitHub project
   'TARGET\_PROJECT\_TITLE' into a JSON file for archival or potential analysis purposes.

   Using the `status` command, you can list the repositories that you have
   already added to your team.

   ```shell
   gh soc status
   ```

6. **If you no longer need the SoC, you can delete it, which entails deleting
   your GitHub organization.**

   ```shell
   gh soc delete
   ```

# The course schedule template

|Date | Topic | Repository for lab & home activities | Submission deadline | Lecture notes|
|:---------------:|:----------------------:|:------------------:|:-------------------------:|:-----:|
|06.10.2023       | Hello R!         | [soc-datascience-hello](https://github.com/pierzcham/soc-datascience-hello)        | 13.10.2023 | [Welcome to data science!](./slides/u1-d01-welcome/u1-d01-welcome.Rmd) <br> [Meet the toolkit: programming](./slides/u1-d02-toolkit-r/u1-d02-toolkit-r.Rmd) <br> [Meet the toolkit: version control & collaboration](./slides/u1-d03-toolkit-git/u1-d03-toolkit-git.Rmd)|
|13.10       | Data visualization        | [soc-datascience-viz](https://github.com/pierzcham/soc-datascience-viz)   | 20.10 | [Data and visualisation](./slides/u2-d01-data-viz/u2-d01-data-viz.Rmd) <br> [Visualising data with ggplot2](./slides/u2-d02-ggplot2/u2-d02-ggplot2.Rmd) <br> [Visualising numerical data](./slides/u2-d03-viz-num/u2-d03-viz-num.Rmd) <br> [Visualising categorical data](./slides/u2-d04-viz-cat/u2-d04-viz-cat.Rmd)|
|20.10       | Data wrangling       | [soc-datascience-wrang](https://github.com/pierzcham/soc-datascience-wrang)   | 27.10 | [Tidy data](./slides/u2-d05-tidy-data/u2-d05-tidy-data.Rmd) <br> [Grammar of data wrangling](./slides/u2-d06-grammar-wrangle/u2-d06-grammar-wrangle.Rmd) <br> [Working with a single data frame](./slides/u2-d07-single-df/u2-d07-single-df.Rmd) <br> [Working with multiple data frames](./slides/u2-d08-multi-df/u2-d08-multi-df.Rmd) <br> [Tidying data](./slides/u2-d09-tidying/u2-d09-tidying.Rmd)|
|27.10       | Spatial data         | [soc-datascience-spatial](https://github.com/pierzcham/soc-datascience-spatial) | 10.11 | [Data types](./slides/u2-d10-data-types/u2-d10-data-types.Rmd) <br> [Data classes](./slides/u2-d11-data-classes/u2-d11-data-classes.Rmd) <br> [Data import](./slides/u2-d12-data-import/u2-d12-data-import.Rmd)|
|10.11       | Effective data visualization          | [soc-datascience-reshape](https://github.com/pierzcham/soc-datascience-reshape) | 17.11 | [Data recode](./slides/u2-d13-data-recode/u2-d13-data-recode.Rmd) <br> [Effective data visualization](./slides/u2-d14-effective-dataviz/u2-d14-effective-dataviz.Rmd)|
|17.11       | Simpson's paradox          |    [soc-datascience-paradox](https://github.com/pierzcham/soc-datascience-paradox)  | 24.11 | [Scientific studies and confounding](./slides/u2-d15-studies-confounding/u2-d15-studies-confounding.Rmd) <br> [Simpson’s paradox](./slides/u2-d16-simpsons-paradox/u2-d16-simpsons-paradox.Rmd) <br> [Doing data science](./slides/u2-d17-doing-data-science/u2-d17-doing-data-science.Rmd)|
|24.11       | Collaboration on Github<br> Work on projects          | [soc-datascience-collabor](https://github.com/pierzcham/soc-datascience-collabor) <br> [soc-datascience-project](https://github.com/pierzcham/soc-datascience-project) | | [Web scraping](./slides/u2-d18-web-scrape/u2-d18-web-scrape.Rmd) <br> [Scraping top 250 movies on IMDB](./slides/u2-d19-top-250-imdb/u2-d19-top-250-imdb.Rmd) <br> [Web scraping considerations](./slides/u2-d20-considerations/u2-d20-considerations.Rmd)|
|01.12       | Web scraping         |     [soc-datascience-scrap](https://github.com/pierzcham/soc-datascience-scrap)  | 08.12 | [Functions](./slides/u2-d21-functions/u2-d21-functions.Rmd) <br> [Iteration](./slides/u2-d22-iteration/u2-d22-iteration.Rmd)|
|08.12       | Ethics and Data Science          |    [soc-datascience-bias](https://github.com/pierzcham/soc-datascience-bias) <br>  project proposals peer reviews | 15.12 | [Misrepresentation](./slides/u3-d01-misrepresentation/u3-d01-misrepresentation.Rmd) <br> [Data privacy](./slides/u3-d02-privacy/u3-d02-privacy.Rmd) <br> [Algorithmic bias](./slides/u3-d03-algorithmic-bias/u3-d03-algorithmic-bias.Rmd)|
|15.12       | Modelling data         |    [soc-datascience-fitting](https://github.com/pierzcham/soc-datascience-fitting)  | 22.12 | [The language of models](./slides/u4-d01-language-of-models/u4-d01-language-of-models.Rmd) <br> [Fitting and interpreting models](./slides/u4-d02-fitting-interpreting-models/u4-d02-fitting-interpreting-models.Rmd) <br> [Modeling non-linear relationships](./slides/u4-d03-modeling-nonlinear-relationships/u4-d03-modeling-nonlinear-relationships.Rmd) <br> [Models with multiple predictors](./slides/u4-d04-model-multiple-predictors/u4-d04-model-multiple-predictors.Rmd) <br> [More models with multiple predictors](./slides/u4-d05-more-model-multiple-predictors/u4-d05-more-model-multiple-predictors.Rmd)|
|22.12       | Classification and model building         |     [soc-datascience-nfitting](https://github.com/pierzcham/soc-datascience-nfitting) | 12.01 | [Logistic regression](./slides/u4-d06-logistic-reg/u4-d06-logistic-reg.Rmd) <br> [Prediction and overfitting](./slides/u4-d07-prediction-overfitting/u4-d07-prediction-overfitting.Rmd) <br> [Feature engineering](./slides/u4-d08-feature-engineering/u4-d08-feature-engineering.Rmd)|
|12.01       | Model validation          |   [soc-datascience-valid](https://github.com/pierzcham/soc-datascience-valid)   | 19.01 | [Cross validation](./slides/u4-d09-cross-validation/u4-d09-cross-validation.Rmd)|
|19.01       | Uncertainty quantification         |     [soc-datascience-hypo](https://github.com/pierzcham/soc-datascience-hypo)   | 26.01 | [Quantifying uncertainty](./slides/u4-d10-quantify-uncertainty/u4-d10-quantify-uncertainty.Rmd) <br> [Bootstrapping](./slides/u4-d11-bootstrap/u4-d11-bootstrap.Rmd) <br> [Hypothesis testing](./slides/u4-d12-hypothesis-testing/u4-d12-hypothesis-testing.Rmd) <br> [Inference overview](./slides/u4-d13-inference-overview/u4-d13-inference-overview.Rmd)|
|26.01       | Text analysis (only lecture notes)  <br> Work on projects      |     [soc-datascience-wrapup](https://github.com/pierzcham/soc-datascience-wrapup)| 02.01| [Text analysis](./slides/u5-d01-text-analysis/u5-d01-text-analysis.Rmd) <br> [Comparing texts](./slides/u5-d02-comparing-texts/u5-d02-comparing-texts.Rmd) <br> [Interactive web apps](./slides/u5-d03-interactive-web-app/u5-d03-interactive-web-app.Rmd) <br> [Machine learning](./slides/u5-d04-machine-learning/u5-d04-machine-learning.Rmd)|
|02.02       | Bayesian inference (only lecture notes)   |     projects presentations |    | [Interactive data visualization](./slides/u5-d05-shiny-1/u5-d05-shiny-1.pdf) <br> [Interactive data visualization and reporting](./slides/u5-d06-shiny-2/u5-d06-shiny-2.pdf) <br> [Bayesian inference](./slides/u5-d07-bayes-inf/u5-d07-bayes-inf.Rmd)|

# Library

- R for Data Science (2e): https://r4ds.hadley.nz/
- Statistical Inference via Data Science: https://moderndive.com/index.html
- Posit Cheatsheets: https://posit.co/resources/cheatsheets/
- Git Guide: https://github.com/git-guides

# Licence

The course materials are madified version of [datasciencebox](https://github.com/tidyverse/datascience-box) under [Creative Commons Attribution-ShareAlike 4.0
International Public License](https://creativecommons.org/licenses/by-sa/4.0/)
