---
description: >-
  In this section, you will find more information about hypothesis in Charles'
  context.
---

# Hypothesis

Hypothesis are **registered alternatives** on the platform to solve some issues or to validate changes of applications that you have integrated on CharlesCD.

Its possible that one hypothesis may have one or more features that are directly related to modules or/and projects that were registered before in your workspace.

Imagine a situation in which two teams work on the same product and have different ideas to raise the client conversion rate. Team A suggests adding a button on the page, meanwhile Team B believes that include a ‘selling suggestion’ box will be more assertive.

![](../.gitbook/assets/hypothesis%20%281%29.png)

Charles makes it possible for both teams to create different hypothesis, so each team is able to lead the development through a board that is automatically created and then they also can independently select different users circles to validate the results of each hypothesis.

## How to create hypothesis?

When you register a hypothesis on Charles, your request will be forwarded to `charlescd-moove`. At the end of this process, the system will create a board and you will be able to manage and create release cards with the necessary actions to test all your hypothesis.

There are two types of cards:

1. **Feature:** cards that involve coding like the new features implementation or fixes on the project. 
2. **Action:** cards that indicate an action to be done, for example, perform a field test with the users. 

![](../.gitbook/assets/ref-hipoteses2%20%282%29.png)

When a feature card is added, Charles creates a new git branch for the client that is directly stored in the used SCM, Git or Bitbucket, for example.

The name of the branch is chosen by the user through **Branch name** field, see below:

## Protected branches 

![](../.gitbook/assets/branch_name.png)

When you delete or alter a card from the **feature** type to the **action** one, the associated branch can be deleted.

If the branch already exists, it is only associated to a card. 

To avoid wrong deletions, Charles allows the **protected branches** configuration. These branches cannot be deleted by Charles.

### Deleting a card

If you delete a card or alter a feature card to a an action one and if the associated branch is on the protected branches list, the deletion branch will be ignored. 

There are three delete card options: 

### Configuration

1. **Archive:**  Only disable the card;
2. **Delete card:** Delete only the card; 
3. **Delete card and branch:** Delete the card and the associated branch. 

You have to configure through environment property `charlescd.protected.branches` on `moove` module. The default value is **master**, **main** and **trunk**. 

## Protected branches 

When you delete or alter a card from the **feature** type to the **action** one, the associated branch can be deleted.

To avoid wrong deletions, Charles allows the **protected branches** configuration. These branches cannot be deleted by Charles.

If you alter a feature card to a an action one and if the associated branch is on the protected branches list, the deletion will be ignored. 

In case you delete a card where the associated branch is configured as protected, when you go to the delete card option, you will see the branch will be disabled. See the image below: 

![](../.gitbook/assets/clipboard-2020-05-10-at-4.10.26-pm.png)

### Configuration

You have to configure through environment property `charlescd.protected.branches` on `moove` module. The default value is **master**, **main** and **trunk**. 

## Board management

The board is organized and structured based on Agile methodology, so you are able to create a backlog with tasks and prioritize what is going to be done \(to do\) and indicate which ones are in progress \(doing\).

As the hypothesis development evolves, the tasks are moved to the next column. The status for each activity are:

* **To Do:** tasks that are prioritized and need to be done;
* **Doing:** tasks in progress;
* **Ready to go:** finished tasks, if it is a feature card, it is possible to generate a build.
* **Builds:** all builds are generated here, after a combination with the features on the previous column. You can expand the cards to have more information. 

Charles is responsible to orchestrate the merges resolution, especially if several same modules cards appears, but with different git ramifications.

After this process is finished and the codes are mixed, a new release ramification is created and the card state is changed to build.

* **Releases Deployed:** the cards on this column show where the hypothesis builds are implemented.

![](../.gitbook/assets/ref-hipoteses%20%281%29%20%281%29.png)

When a hypothesis is moved to the **Ready to Go** column, you indicate to the system that a specific card can go through a **Generate Release Candidate** process, which means the hypothesis will transform into a branch release of the master release on your git.

