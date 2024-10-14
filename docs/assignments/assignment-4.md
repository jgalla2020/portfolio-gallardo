---
title: Assignment 4
layout: doc
---

# Assignment 4: Backend Design and Implementation

<h2 align="center">
    Design Reflection
</h2>

I received great feedback from my data models in A3. My main additions where the Sessioning and Authenticating concepts since the rest of my concepts depended on the user being authenticated and/or having an active session. I also realized that many of my concepts had redundancies, particularly Sharing and DirectMessaging, so I combined these into one Messaging concept since they both dealt with sending some information to another user. I also generalized the TaskSetting concept to an Itemizing concept that allows the user to create arbitrary items with descriptions attached. My reasoning was that this concept could be used for many things, including creating tasks. Moreover, given that this is a social media app, I decided to add a Profiling concept for the user to make their profile where they can eventually describe their preferences, as well as other useful details for other users to see.

<h2 align="center"> 
    Abstract Data Models
</h2>

### Itemizing [Creator]

```
State Components
    Entities
        items: Item

    Relations
        createdItems: Creator -> Item

State Methods
    createItem(creator: Creator, item: Item)
        createdItems' = createdItems U {(creator, item)}

    updateItem(creator: Creator, oldItem: Item, newItem: Item)
        if (creator, oldItem) not in createdItems:
            throw an error

        createdItems' = createdItems - {(creator, oldItem)}
        createdItems' = createdItems U {(creator, newItem)}

    deleteItem(creator: Creator, item: Item)
        createdItems' = createdItems - {(creator, item)}

    getItems()
        return all items

    getItemsByCreator(creator: Creator)
        return createdItems for creator
```

### Messaging [Contact]

```
State Components
    Entities
        messages: TextMessage

    Relations
        sentMessages, queuedMessages: Contact -> TextMessage

State Methods
    createDraftMessage(contact: Contact, message: TextMessage, attachment?: Attachment)
        queuedMessages' = queuedMessages U {(contact, message, attachment)}

    editDraftMessage(contact: Contact, oldMessage: TextMessage, newMessage: TextMessage, attachment?: Attachment)
        if (contact, oldMessage) not in queuedMessages:
            throw an error

        queuedMessages' = queuedMessages - {(contact, oldMessage)}
        queuedMessages' = queuedMessages U {(contact, newMessage, attachment)}

    deleteDraftMessage(contact: Contact, message: TextMessage)
        queuedMessages' = queuedMessages - {(contact, message)}

    sendMessage(contact: Contact, message: TextMessage)
        if (contact, message) not in queuedMessages:
            throw an error

        queuedMessages' = queuedMessages - {(contact, message)}
        sentMessages' = sentMessages U {(contact, message)}

    editSentMessage(contact: Contact, oldMessage: TextMessage, newMessage: TextMessage, attachment?: Attachment)
        if (contact, oldMessage) not in sentMessages:
            throw an error

        sentMessages' = sentMessages - {(contact, oldMessage)}
        sentMessages' = sentMessages U {(contact, newMessage, attachment)}

    deleteSentMessage(contact: Contact, message: TextMessage)
        sentMessages' = sentMessages - {(contact, message)}

    readDraftMessages(contact: Contact)
        return queuedMessages for contact

    readSentMessages(contact: Contact)
        return sentMessages for contact

    readReceivedMessages(contact: Contact)
        return sentMessages where contact is the recipient
```

### Tracking [Executor]

```
State Components
    Entities
        goals: Goal

    Relations
        createdGoals: Executor -> Goal

State Methods
    createGoal(executor: Executor, goal: Goal)
        createdGoals' = createdGoals U {(executor, goal)}

    viewGoals(executor: Executor)
        return createdGoals for executor

    viewStatus(executor: Executor, status: GoalStatus)
        return createdGoals for executor with status = status

    viewGoalById(goalId: Goal)
        return goal with goalId

    updateGoal(executor: Executor, oldGoal: Goal, newGoal: Goal)
        if (executor, oldGoal) not in createdGoals:
            throw an error

        createdGoals' = createdGoals - {(executor, oldGoal)}
        createdGoals' = createdGoals U {(executor, newGoal)}

    deleteGoal(executor: Executor, goal: Goal)
        createdGoals' = createdGoals - {(executor, goal)}

    updateGoalStatuses()
        change status of past due goals
```

### Profiling [User]

```
State Components
    Entities
        profiles: Profile

    Relations
        userProfiles: User -> Profile

State Methods
    createProfile(user: User, profile: Profile)
        userProfiles' = userProfiles U {(user, profile)}

    viewProfile(user: User)
        return userProfiles for user

    updateProfile(user: User, updatedProfile: Profile)
        if (user, oldProfile) not in userProfiles:
            throw an error

        userProfiles' = userProfiles - {(user, oldProfile)}
        userProfiles' = userProfiles U {(user, updatedProfile)}

    deleteProfile(user: User)
        userProfiles' = userProfiles - {(user, profile)}

```

<h2 align="center"> 
    Concept Implementations
</h2>

Please go to this link to my repo: https://github.com/jgalla2020/61040-A4.

<h2 align="center"> 
    Deployment of Web Service
</h2>

Please go to my Vercel deployment: https://61040-a4-e27u7wz8e-jabes-gallardos-projects.vercel.app/.
