---
title: Assignment 3
layout: doc
---

# Assignment 3: Convergent Design

<h2 align="center"> 
    Product Pitch 💡
</h2>

<!-- TODO: find way to delete comment -->

Seek-N-Find is a mentorship-driven social media app meant for goal-oriented individuals seeking one-on-one mentor-mentee connections for life and spiritual matters. Many social media platforms entice users to make connections without effectively motivating them to cultivate fruitful relationships. To solve this, Seek-N-Find combines matchmaking capabilities, the organization and reminders offered by calendar tools, and real-time collaboration features to foster genuine, long-lasting human connections in a concise, streamlined manner.

This app allows users, both mentors and mentees, to specify their general availability, mentorship goals, and connection preferences and eligibility requirements in a standardized format within their profile. Users can search for mentors and mentees by browsing through categories and sub-categories provided in the app or by conducting filtered searches. They can view others' profile information and express interest in a mentor or mentee by sending a connection request if they are eligible. If they are not eligible due to the mentor setting up certain mentee requirements in their profile, the prospective mentee can export a plan of preparation to eventually become more eligible. Upon expressing mutual interest, two users can form a mentor-mentee match. They can then begin direct messaging each other, schedule introductory calls in the app or collaboratively write up agendas and long-term goals using the task assignment feature, among other tools.

<h2 align="center">
    Functional Design ⚙️
</h2>

### Concepts

1. **Recording[Info, Medium]**

   - **Purpose**: record information on some medium
   - **Operational Principle**: after recording information on some medium, the user can view what they have recorded, make edits, and save those edits. The user can also delete the entire piece of information or a part of it.
   - **State**:
     - const information: Info
   - **Actions**:
     - record(information: Info, medium: Medium)
     - view(out information: Info)
     - edit(commands: List)
     - delete()

2. **Sharing[Item, Address]**

   - **Purpose**: share an item to an address
   - **Operational Principle**: the user selects an item, specifies a recipient address to share the item with. The user can view the other users with whom they have shared the item and remove or add users if they would like to.
   - **State**:
     - const item: Item
     - const address: Address
   - **Actions**:
     - select(item: Item)
     - addAddress(address: Address)
     - share(address: Address)
     - removeAddress(address: Address)

3. **Messaging[Recipient]**

   - **Purpose**: directly message a recipient
   - **Operational Principle**: after a user writes a text message and does not delete it, they can send it directly to a recipient. The user can view, edit or delete the message.
   - **State**:
     - const textMessage: Message
     - const recipient: Recipient
   - **Actions**:
     - write(textMessage: Message)
     - edit(commands: List)
     - delete()
     - send(recipient: Recipient)

4. **TaskSetting[User]**

   - **Purpose**: assign a task for some other user to complete
   - **Operational Principle**: the user specifies the details of the task like the description and due date, and assigns that task to another user. The user who created and assigned the task can check back in to modify the task specification, due date, or persons assigned.
   - **State**:
     - const users: set User
     - const tasks: set Task
     - const assignedTo: task --> set User
   - **Actions**:
     - specify(details: String)
     - assign(user: User)
     - unassign(user: User)
     - edit()
     - complete(out task: Task)

5. **Tracking[Item]**

   - **Purpose**: track the completion of some item with a due date
   - **Operational Principle**: the user selects an item and sees the percentage completed and how much time left until it is due.
   - **State**:
     - const items: set Item
     - const status: set Status
     - const progressOf: item --> status
   - **Actions**:
     - select(item: Item)
     - getInfo(item: Item, out status: Status)

6. **Requiring[Criteria, Privilege]**

   - **Purpose**: provide required criteria to obtain some privilege
   - **Operational Principle**: user specifies a set of criteria that other users must meet to receive a privilege. The user can modify these criteria or privilege.
   - **State**:
     - const criteria: set Criteria
     - const privilege: Privilege
   - **Actions**:
     - initialize()
     - addCriteria(criteria: Criteria)
     - deleteCriteria(criteria: Criteria)
     - changePrivilege(newPrivilege: Privilege)
     - publish(out set Criteria)

7. **Qualifying[Privilege]**

   - **Purpose**: determine a plan for how to qualify for some privilege
   - **Operational Principle**: after selecting a privilege, the user receives a step-based plan to complete to access the privilege. This plan is saved for the user to review later, as well as to create tasks out of this plan.
   - **State**:
     - const plan: set Task
     - const active: set Task
     - const privilege: Privilege
   - **Actions**:
     - getPlan(privilege: Privilege, out plan: set Task)
     - addTask(task: Task)
     - removeTask(task: Task)

### App-Level Actions and Synchronization

```
app SeekNFind
    ...including other standard concepts...

    include Recording[Text, Textbox], Sharing[Task, Email], Messaging[User]
    include TaskSetting[User], Tracking[Task]
    include Requiring[Criteria, Connection], Qualifying[Connection]

    sync sendMessage(textMessage: String, recipient: Recipient)
        Messaging.write(textMessage)
        Messaging.send(recipient)

    sync setCriteria(criteriaList: set Criteria)
        Requiring.initialize()

        for criteria in criteriaList:
            Requiring.addCriteria(criteria)

    sync getEligibilityPlan(privilege: Privilege)
        Qualifying.getPlan(privilege)

    sync createTask(details: String)
        TaskSetting.specify(details)

    sync trackTask(task: Task)
        Tracking.select(task)
        Tracking.getInfo(task)

    sync assignTask(user: User)
        TaskSetting.assign(user)
```

### Dependency Diagram

<div align="center">
  <img src="/../assets/images/dependency_diagram.png" alt="dependency_diagram" width="500">
</div>

<h2 align="center">
    Wireframes 📒
</h2>

<!-- <iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="688" height="450" src="https://embed.figma.com/design/Y5Pv0tcc4fsD4EhETAW5zv/A3-Wireframe?node-id=11-96&embed-host=share" allowfullscreen></iframe> -->

<h2 align="center"> 
    Design Tradeoffs ⚖️
</h2>

### Mentor-Mentee Connection Model

- **Options**:
  1. Allow only mutual connections without the ability for the user to create requirements.
  2. Allow only mutual connections but with the ability to create requirements and to become eligible to meet them.
  3. No requirements or mutual connections; allowing one-sided requests like in mainstream social media.
- **Rationale**: This increases the likelihood of the mentee taking the necessary steps to prepare for the mentorship, and reduces the chances of the mentor being reached out to by unqualified mentees. This app is meant to foster committed and recurring one-on-one connections, which is not accomplished as effectively through one-sided requests. This might seem restrictive if it prevents certain connections, but that is why there is the option to edit requirements and "level up" based on the eligibility plan provided.

### Mentor/Mentee Search

- **Options**:
  1. Provide advanced filtering and ability to create custom filters near the search bar.
  2. Lay out the general categories and sub-categories of the types of mentors offered.
  3. Only allow keyword search.
- **Rationale**: As a user that prefers designs that lend themselves to exploration and well-informed development of preferences, I chose option 2 because it gives the user a good idea of what they can find on the app without knowing exactly what they are looking for. This also allows the user to narrow down their desired choices by clicking through categories that go from general to specific. Though advanced filtering is great for users with highly specific needs, option 2 is a good starting point.

### Task Creation and Sharing

- **Options**:
  1. Allow users to jot down bullet points in a generic to-do list just for themselves.
  2. Allow users to create task items with a due date option; it is just for themselves.
  3. Allow users to create task items with a due date option; they can share and assign them to others in their network.
- **Rationale**: Option 2 and 3 supersede option 1 because they provide a due date option and make more concrete the notion of a task that the user must complete separate from other tasks. I chose option 2 because I mentors to be able to assign tasks to their mentees in a way where they can both keep track of the progression.
