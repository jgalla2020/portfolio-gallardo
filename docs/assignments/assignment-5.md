---
title: Assignment 5
layout: doc
---

# Assignment 5: Frontend Design & Implementation

_I used ChatGPT to help me debug and to suggest code for concepts that I was unfamiliar with._

## Heuristic Evaluation and Writeup

### 1. Usability Heuristics

- **Discoverability**: I include big titles at the top of some pages giving the user a quick idea of what to do. The homepage title is "Explore new mentors", and the task page title is "Create new tasks for your workstation." There are also subtitles below these for further detail if the user needs more information. I also include a standard sidebar navigation feature that the user accesses by clicking on the hamburger menu, which when opened gives five titles for pages that the user can navigate to by clicking on the buttons. The user can open this sidebar at any time to discover new sections. I also include titled buttons with icons to give the user a good idea of what page the button leads to. In each profile page, the connection buttons are pretty evident.

  **Suggested Improvement**: Perhaps there could be a clearer way for the user to discover the Set Criteria feature. As it stands, the user has to go into their profile to set this up. There are pages out there like Handshake in which the user is initially led through a setup where they specify career preferences and so forth, so adopting something similar might be beneficial.

- **Efficiency**: Each page of my wireframe uses a grid structure that allows the user to easily browse different mentorship categories and mentor profiles, each of which have compact rectangular-shaped previews with only essential information for the user to make a decision. For example, the homepage presents the user with six mentorship categories with simple titles and small icons. The user can click any of these buttons to explore sub-categories and narrow down preferences. Nevertheless, if the user wishes to switch between different top-level categories, my current design only allows them to navigate back to the original categories. This could be inefficient if the user changes their mind.

  **Suggested Improvement**: Place the mentorship categories laid out in the home page right below the search bar so that a user can easily change between top-level categories. This alone might not be conducive to being able to switch between lower-level categories as efficiently, but the assumption is that the categories will not go unreasonably deep.

### 2. Physical Heuristics

- **Perceptual Fusion**: My interface design currently does not account for time delays. If the user was getting the plan to become eligible to connect with another user, there are no indicators that the task is being done in the backend. There is not much need for these indicators when navigating between pages using the sidebar navigation since the expectation is that it will be quicker.

  **Suggested Improvements**: Adding progress bars or loading screens in between clicking buttons and getting results would be a good addition.

- **Situational Context**: While my wireframes make good use of page titles for this, I need to include more standardized ways for the user to check where they are at. Some pages don't have such titles and even if they did it would be aesthetically displeasing. The user evidently knows that they are logged in since their icon appears at the top right corner of the page. When the user is specifying preferences or creating tasks, I make use of pop ups for the user to fill out the information, which is a clear way of showing the user where they are at in those respective processes.

  **Suggested Improvements**: In the sidebar, I could bold or highlight the section that the user is in. I could also standardize the titles across all pages, while also adding breadcrumbs at the top of the page where appropriate (i.e., exploring mentorship categories, mentorship profiles).

### 3. Linguistic Heuristics

- **Speak a User's Language**: My wireframes make extensive use of titles, subtitles, and minor descriptions for major sections of the app. I also use icons where appropriate and label important buttons so that the users have both visual cues and written confirmation.

  **Suggested Improvements**: As of now, beyond adding more standardization, I do not have major suggestions since my wireframe incorporates a good variety of helpful informative messages.

- **Consistency**: I definitely use the same profile structure for both the previews and the profiles themselves, which will help the user focus on details that differentiate one profile from the other. I also always keep the top search bar and the sidebar navigation so that the user has it as an anchor. I admit that there is not a lot of stylistic consistency between the home page, the profile browsing page, and the task page. This is likely due to taking inspiration from the design of highly different apps.

  **Suggested Improvements**: Find a consistent design between the task page and the profile page, given that mentors will be assigning tasks to their mentees.

### Tradeoffs

I definitely sacrifice **discoverability** to boost **efficiency** in the suggestion to lay out the mentorship categories below the search bar. This might not have the same feeling of going down a rabbit hole, as well as presenting big buttons with nice icons, but it would give the user faster switching ability. Likewise, adding standardization for situational context might increase **consistency** but might not **speak the same language** of informally greeting the user in different fashions.

## Set of Components for Profiling Concept

You can view these components in my [GitHub repo](https://github.com/jgalla2020/A5).

## Attempt at Frontend Deployment

Go [here](https://61040-a5-46gnmck5z-jabes-gallardos-projects.vercel.app) to view my deployment.
