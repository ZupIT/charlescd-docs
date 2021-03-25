---
description: 'In this section, you will find more information about Users Groups on Charles.'
---

# Users Groups

A user group can represent a team or even a subset of people based on their skills.

For example, a big team would have different Charles access permissions according to their job position, you could have these groups:

* _Product X developers; Desenvolvedores do Produto X._
* _Product X QAs;_
* _Product X Data Analysts._

However, if everyone in a team has the same permission, you are able to create only one group with its users.

* _Product X Team._

![Preview of User Group &quot;Data Analysts do Produto X&quot;](../.gitbook/assets/image%20%283%29%20%282%29.png)

## Permissions for user groups in your workspace

Charles permissions are given to a user group when you associate them with a workspace.

The following profiles are available:

* **Maintainer**: can access and edit all workspace configurations. They can do implementations and create, edit and delete circles and modules. 
* **Developer**: they have access to do implementations

  and also create, edit and delete circles and modules.

* **Analyst**: they have permission to edit and delete circles and modules. And also view the modules' configuration.
* **Reader**: is able to view circles and modules.

![Permission options to associate users&apos; groups on a workspace.](../.gitbook/assets/chrome-capture-3-%20%282%29.gif)

### Permissions map

See below the permission given to each profile:

| Modules | Action | Root | Maintainer | Developer | Analyst  | Reader |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **Users** | Create | ✔  |   |   |   |   |
|   | Edit | ✔  |   |   |   |   |
|   | Delete | ✔  |   |   |   |   |
|   | View | ✔  |   |   |   |   |
| **User Groups** | Create | ✔  |   |   |   |   |
|   | Edit | ✔  |   |   |   |   |
|   | Delete | ✔  |   |   |   |   |
|   | View | ✔  |   |   |   |   |
| **Workspace** | Create | ✔  |   |   |   |   |
|   | Configure | ✔ | ✔ |   |   |   |
|   | Delete | ✔  |   |   |   |   |
|   | View | ✔  | ✔  | ✔  | ✔  | ✔  |
| **Circle** | Create/Edit/Delete | ✔  | ✔  | ✔  | ✔  |   |
|   | View | ✔  | ✔  | ✔  | ✔  | ✔  |
|   | View | ✔  | ✔  | ✔  | ✔  | ✔  |
| **Modules**  | Create/Edit/Delete | ✔  | ✔  | ✔  |   |   |
|   | View | ✔  | ✔  | ✔  | ✔  | ✔  |
| **Deploy**  | Make deployments | ✔  | ✔  | ✔  |   |   |

