---
title: Assignment 4
layout: doc
---

# Assignment 4: Backend Design and Implementation

<h2 align="center"> 
    Abstract Data Models
</h2>

_The header titles are the names of my concepts. Based on the A3 feedback, I've modified my concepts._

### Recording

```
State Components
    Entities
        mediums: set ContentType

    Relations
        finished, inProgress: Content -> ContentType

State Methods
    beginRecord(content: Content, type: ContentType)
        inProgress' = inProgress U {(content, type)}

    saveRecord(content: Content, type: ContentType)
        inProgress' = inProgress - {(content, type)}
        finished' = finished U {(content, type)}

    beginEdit(record: Record)
        finished' = finished - {(record.content, record.type)}
        inProgress' = inProgress U {(record.content, record.type)}

    makeEdit(oldRecord: Record, newContent: Content)
        inProgress' = inProgress - {(oldRecord.content, oldRecord.type)}
        inProgress' = inProgress U {(newContent, oldRecord.type)}
```

### Sharing

```
State Components
    Entities
        types: set ItemType
        addresses: set Address

    Relations
        items: Title -> ItemType
        shareTo, sharedWith: Item -> set Address

State Methods
    createItem(title: Title, type: ItemType)
        items' = items U {(title, type)}

    addAddress(item: Item, address: Address)
        addresses' = addresses U {address}
        shareTo' = shareTo U {(item, address)}

    removeAddress(address)
        addresses' = addresses - {address}

    shareItem(item: Item, address: Address)
        shareTo' = shareTo - {(item, address)}
        sharedWith' = sharedWith U {(item, address)}

    unshareItem(item: Item, address: Address)
        sharedWith' = sharedWith - {(item, address)}
```

### DirectMessaging

```
State Components
    Entities
        contacts: Contact

    Relations
        sentMessages, queuedMessages: Contact -> TextMessage

State Methods
    addContact(contact: Contact)
        contacts' = contacts U {contact}

    deleteContact(contact: Contact)
        contacts' = contacts - {contact}

    createMessage(contact: Contact, message: TextMessage)
        queuedMessages' = queuedMessages U {(contact, message)}

    changeRecipient(oldContact: Contact, newContact: Contact, message: TextMessage)
        if (oldContact, message) in sentMessages:
            throw an error

        queuedMessages' = queuedMessages - {(oldContact, message)}
        queuedMessages' = queuedMessages U {(newContact, message)}

    sendMessage(contact: Contact, message: TextMessage)
        if (contact, message) not in queuedMessages:
            throw an error

        queuedMessages' = queuedMessages - {(contact, message)}
        sentMessages' = sentMessages U {(contact, message)}

    unsendMessage(contact: Contact, message: TextMessage)
        if (contact, message) not in sentMessages:
            throw an error

        sentMessages' = sentMessages - {(contact, message)}

    editQueuedMessage(contact: Contact, oldMessage: TextMessage, newMessage: TextMessage)
        if (contact, oldMessage) not in queuedMessages:
            throw an error

        queuedMessages' = queuedMessages - {(contact, OldMessage)}
        queuedMessages' = queuedMessages U {(contact, newMessage)}

    editSentMessage(contact: Contact, oldMessage: TextMessage, newMessage: TextMessage)
        if (contact, oldMessage) not in sentMessages:
            throw an error

        sentMessages' = sentMessages - {(contact, OldMessage)}
        sentMessages' = sentMessages U {(contact, newMessage)}
```

### TaskSetting

```
State Components
    Entities
        users: set User
        tasks: set Task

    Relations
        assignable, assigned: User -> set Task

State Methods
    addTask(task: Task)
        tasks' = tasks U {task}

    deleteTask(task: Task)
        tasks' = tasks - {task}

    editTask(task: Task, updated: Description)
        thisTask = tasks.get_task(task)
        thisTask.info = updated

    markComplete(task: Task)
        thisTask = tasks.get_task(task)
        thisTask.status = completed

    addUser(user: User)
        users' = users U {user}

    addUserToTask(user: User, task: Task)
        assignable' = assignable U {(user, task)}

    deleteUser(user: User)
        users' = users - {user}

    assignTask(task: Task, user: User)
        assignable' = assignable - {(user, task)}
        assigned' = assigned U {(user, task)}

    unassignTask(task: Task, user: User)
        assigned' = assigned - {(user, task)}
```

### TrackingItems

```
State Components
    Entities
        items: set Item
        statusTypes: set StatusType

    Relations
        statusOf: Item -> Status
        incomplete: Item -> TimeStamp
        completed: Item -> TimeStamp

State Methods
    addItem(item: Item, status: Status)
        items' = items U {item}
        statusOf' = statusOf U {(item, status)}

    deleteItem(item: Item)
        items' = items - {item}

        status = stausOf[item]
        statusOf' = statusOf - {(item, status)}

    checkStatus(item: Item)
        return statusOf[item]

    changeStatus(item: Item, oldStatus: Status, newStatus: Status)
        statusOf' = statusOf - {(item, oldStatus)}
        statusOf' = statusOf U {(item, newStatus)}

    getComplete()
        return {(item, timestamp) for item, timestamp in complete}

    getIncomplete()
        return {(item, timestamp) for item, timestamp in incomplete}
```

### Requiring

```
State Components
    Entities
        privileges: set Privilege

    Relations
        requirements: Privilege -> set Criteria

State Methods
    addPrivilege(privilege: Privilege)
        privileges' = privileges U {privilege}

    removePrivilege(privilege: Privilege)
        privileges' = privileges - {privilege}
        delete requirements[privilege]

    addRequirement(privilege: Privilege, requirement: Criteria)
        requirements[privilege] = requirements[privilege] U {requirement}

    removeRequirement(privilege: Privilege, requirement: Criteria)
        requirements[privilege] = requirements[privilege] - {requirement}

    editRequirement(privilege: Privilege, oldRequirement: Criteria, newRequirement: Criteria)
        requirements[privilege] = requirements[privilege] - {oldRequirement}
        requirements[privilege] = requirements[privilege] U {newRequirement}
```

### Qualifying

```
State Components
    Entities
        desired: set Privilege
        qualified: set Privilege

    Relations
        plans: Privilege -> set Task

State Methods
    canAccess(privilege: Privilege)
        return True if able to access privilege, False otherwise

    addDesiredPrivilege(privilege: Privilege)
        desired' = desired U {privilege}

    generateTasks(privilege: Privilege)
        return tasks based on requirements in privilege

    startSeeking(privilege: Privilege)
        desired' = desired U {privilege}

        tasks = generateTasks(privilege)
        plans' = plans U {(privilege, tasks)}

    stopSeeking(privilege: Privilege)
        desired' = desired - {privilege}
        delete plans[privilege]

    claimPrivilege(privilege: Privilege)
        if plans[privilege] is completed:
            delete plans[privilege]
            desired' = desired - {privilege}
            qualified' = qualified U {privilege}
        else:
            throw message of pending requirements to be met

    renouncePrivilege(privilege: Privilege)
        qualified' = qualified - {privilege}
```

<h2 align="center"> 
    Working Implementation of Two Concepts
</h2>

Please go to this link to my repo: https://github.com/jgalla2020/61040-A4.

I unfortunately did not complete this. I look forward to finishing it for the final submission!

<h2 align="center"> 
    Initial Deployment of Web Service
</h2>

I unfortunately did not complete this. I look forward to finishing it for the final submission!

<h2 align="center"> 
    Outline of Route Design
</h2>

I unfortunately did not complete this. I look forward to finishing it for the final submission!
