# Development Workflow

We want to fine-tune the development workflow to have the right balance of stability and speed.

###  Development Ticket Creation \(Feature or Bug\):

Everyone on the team can create new tickets.

**Steps:**

1. Create a Ticket with pending labels in the backlog
2. Assign the ticket to a technical lead for confirmation

* Assign to Project Manager or Head of Technology if their inputs needed

1. The ticket would be discussed and we will make sure that the description is clear or any extra information should be added.

* For tickets with the type of Bug we must have :
  * Steps to reproduce
  * Expected Result
  * Actual Result
* For tickets with the type of feature we must have:
  * Detailed Description
  * The purpose of the feature
  * Extra resources \(if needed\)

###  Ticket Development

1. During Sprint meetings we will discuss and assign different tasks to different Developers for the next milestone
2. When a developer wants to start the task he/she should change the state to `in progress`
3. For each task developers must create a separate branch with the following structure:

* `<issue type>/<issue number on gitlab>-<issue description>`
* Example: feature/271-fix-login

1. For each commit developers must use the following structure for the commit message:

* `#<issue number> <commit message>`
* Example: \#271 change login page style

1. After completing the task developer must create an MR with the following title:

* `<task type> #<issue number> <task title>`
* Example: feature \#271 login page style

1. After creating MR, both issue and MR should be assigned to another developer for code review and the label should be `in review`
2. After code review the MR should be approved and merged then the assignee should be changed to **riledsun** in order to deployment \(we can ignore this step by having scheduled deployments\)
3. When the changes have been deployed the status should be changed to `in testing` and the assignee should be changed to the Test team.
4. If the task was working properly then it should be closed and the status should be changed to `completed`
5. In any of the above steps on the rejection of that task the assignee should be changed into the developer and status should be changed into `pending`

###  Some Useful Hints:

* Each developer must have less than 3 tickets with a state of `in progress`
* Each Ticket must have only one assignee unless it needs discussion or a teamwork action.

