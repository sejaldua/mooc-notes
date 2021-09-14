# agile-coursera

Agile with Atlassian Jira

## Week 1

### What is Agile?

- approach to managing and working on projects
- simple approach to managing complexity
- example projects include
  - software development
  - managing service tickets
  - HR new hire process
  - managing tasks at a picnic
  - writing a book

### Characteristics

1. **Incremental**: plan, development, and release increments
2. **Iterative**: continuously improve the product and the process of builing the product
3. **Focus on value**
4. **An empowered team**: team consists of distributed brain

### Iterative Learning Loops

- scientific method is the foundation of many processes
- examples
  - plan >> build >> release
  - plan >> do >> check >> act / adjust
  - think >> build >> ship >> tweak

### Agile vs Waterfall

#### Waterfall

- plan and develop the whole project in phases
- Analyze (use case / requirements) >> Design (spec) >> Build >> Test (QA) >> Release
- effectively "one cycle" of the scientific method
- downsides
  - big up-front plan will be wrong because:
    - can't predict the future
    - high value features aren't correctly identified; you will build many unnecessary features
    - things are harder, more problematic, and take longer to build than you think
    - market and/or team will change while you build
  - change is hard and expensive
  - create a lot of obsolete documents
  - feedback is drastically delayed
- why waterfall?
  - when you have high setup costs
  - when the work is predictable
- why is waterfall outdated in many cases?
  - setup cost of many phases is trending towards zero
  - provides little continous feedback

#### The Difference

- one giant "iteration" of scientific method for waterfall; many small iterations for agile
- waterfall is sequential; agile is concurrent
- waterfall is command and control process; agile is distributed process

### Jira Overview

Jira = software used to help manage, develop, and communicate about work

- **application level**: Jira = a collection of projects
  - **project level**: Project = a collection of issues
    - **issue level**: Issue = a work item

### Visualizing Work

#### To Do List

- visually reminds you
- focuses you
- sets priorities
- tracks projects

#### Boards

- board is an agile tool used to visualize and manage work
- kanban board, scrum board, task board, project board
- 2-dimensional to-do list

#### Why Visualize Work?

- easily see the work of the project
  - allows anyone to see the (true) current state of the project
  - organizes and focuses the team
  - only work on tasks on the board
- manage things
  - easy to add and prioritize the work of the project
  - easy to update work items
- improve the team's way of working by visually identifying problems

#### Workflows

- set of columns of a board represent a workflow for completing the work of an issue
- terms used interchangeably are workflow, process, business process, and valuestream
- workflows are broken down into steps (or statuses, states, or stages)

### Kanban Overview

- Agile = mindset
- Common methods include kanban, scrum, XP, etc
  - each embody core principles of agile
- Kanban = an agile method used to manage a continuous queue of work items
  - commonly used ideas
    - limit work in progress
    - remove bottlenecks to improve "flow" of value
    - pull work rather than push work
- Why choose Kanban Method?
  - very lightweight and efficient
  - evolutionary approach of transforming to agile
  - works well if the workflow is service-oriented
    - operations
    - support
    - maintenance development
    - new hire funnel
  - supports multi-team and multi-project workflows
- in Jira, the kanban backlog can be separated from the kanban board, simplifying the kanban board and allowing separate backlog work

#### Limit Work in Progress (WIP)

- How?
  - Specify the minimum and/or maximum number of issues allowed in certain Kanban board columns
- Why?
  - Better flow
  - Limits waste

#### Pulling vs Pushing Work

- performers either push work to the next step or pull from the previous work step

#### Agile Reports

- advantages
  - visualize the work
  - promote transparency
  - aid troubleshooting and continuous improvement
  - aid planning and estimating
- popular report: Cumulative Flow Diagram
  - shows number of issues in each status over time
- another report: Cycle Time Control Chart
  - lead time = time from issue creation to completion
  - cycle time = time from starting work on an issue to completion

## Week 2

### Scrum Overview I - Artifacts

- Scrum = a framework for developing, delivering, and sustaining complex products
- a way of achieving agility
- continuous learning
  - start with vision and actualize the vision iteratively over time

- increment = usable product that may be given to the customer
  - meets the organization's "definition of done"
  - contains the work of the current iteration as well as all prior iterations
- sprint = time-boxed period used to work on an increment of the product
  - usually 1-4 weeks (typically 2 weeks)

### Parts of the Scrum Framework

- **Artifacts**
  - Why?
    - provide project transparency
    - enable shared understanding
    - enable inspection and adaptation
  - Terminology
    - product backlog = ordered, ever-changing to do list for the project
      - contains issues, items, stories
      - can include features, improvements, bug fixes, etc.
      - issues near the top should include more detail
      - modifying the product backlog is called product backlog refinement
    - sprint backlog = list of issues to be completed in the sprint
      - includes the plan on how to accomplish the work of the issues
      - estimation: story points
        - story points are a relative measure of the amount of work required to complete the story
          - fibonacci methodology works to categorize complexity (1, 2, 3, 5, 8, ...)
    - sprint goal = represents the objective of the sprint's increment
      - reached by completing the sprint backlog
      - does not change during the sprint
      - sprint is a success if the sprint goal is reached
      - why have a sprint goal?
        - 1. provides coherence to the product increment
        - 2. enables flexibility with the sprint backlog
    - sprint board = only contains issues from the sprint backlog
      - often called kanban boards
    - reports
      - why agile reports?
        - visualize the work
        - promote transparency
        - aid troubleshooting and continuous improvement
        - aid planning and estimating
      - common scrum reports
        - *burndown chart*: shows the progress that the team makes during a sprint
        - *sprint report*: summary of the sprint (generic)
        - *velocity chart*: shows the estimated and actual velocity of the team over time
          - velocity = rate at which team accomplishes work (units vary) per sprint

- **Roles**
  - Why separate roles?
    - divide and conquer
    - checks and balances
  - Scrum Team: cross-functional, flexible / adaptable, self-organizing
    - product owner
      - responsible for communicating the product vision
      - maximizing the value of each increment
      - the product backlog
      - interacts with, represents, and is accountable to stakeholders
    - scrum master
      - responsible for promoting and supporting scrum
      - improving the day-to-day effectiveness of the team
      - protecting the focus of the team
      - increasing the transparency of the project
      - typical tasks:
        - coaching the scrum team and stakeholders on scrum
        - removing blocking issues
        - facilitating scrum events
        - configuring scrum artifacts
        - monitoring sprint progress
    - development team members (3-9 members)
      - cross-functional, adaptive team that does the work of the project
      - responsible for:
        - estimating issue time
        - deciding how much work can be done in a sprint
        - deciding how to organize to do the work of the sprint
        - creating the increment of each sprint
        - ability to modify the sprint backlog during the sprint
  - stakeholders = others interested in the success of the project
    - internal: company managers, executives, other scrum teams
    - external: customers, partners, investors
- **Events / Meetings**
  - common characteristics of all scrum meetings
    - fixed maximum time limit, no minimum time limit
    - meetings are primarily to *plan, inspect, and adapt*
    - primarily about collaborating, not about updating status
    - primarily spend time on things of value to all participants
  - meetings
    - sprint planning meeting
      - **attendees**: entire scrum team
      - **duration**: typically 4 hours for a 2 week sprint
      - **purpose**: plan the work of the sprint
      - **output**: sprint goal, sprint backlog
    - daily standups / scrum
      - **attendees**: develop team (primarily)
      - **duration**: 15 minutes
      - **purpose**: inspect recent progress toward sprint goal, plan the day's work, identify any impediments, and plans to resolve them
      - **output**: plan for the day
    - sprint review
      - **attendees**: scrum team and stakeholders
      - **duration**: typically 2 hours for a 2 week sprint
      - **purpose**: inspect the increment and collaboratively update the product backlog
      - **output**: first-pass next sprint backlog
    - sprint retrospective
      - **attendees**: scrum team
      - **duration**: typically 90 minutes for a 2 week sprint
      - **purpose**: the team inspects itself, including its processes, tools and team interaction (*this is a positive meeting*)
      - **output**: improvement issue(s) added to the next sprint's backlog

### Toyota Kanban

#### Toyota's Simplified History

- "catch up with America in 3 years"
- eliminate waste and increase productivity
- embraced ideas from Ford, but used a more "agile" approach

#### What is a Kanban?

- Kanban = an object that controls the flow of work
- idea came to Toyota from supermarkets
  - instead of push, order when inventory is low (pull)
    - sometimes called "just in time" system
  - matches the supply and demand
  - empty box is acting as a "kanban" - a signal to order more
- other examples of kanbans
  - guest check
  - empty coffee cup
  - jira issue

#### Kanban Systems

> *"Toyota production system is the production method and the kanban system is the way it is managed."*

Benefits:

- visualizes work
- simple
- reliable
- efficient
- eliminates waste
- identifies bottlenecks / easy to improve

#### Kanban Definitions

- Kanban token = an object that controls the flow of work
- Kanban system = a system that controls the flow of work
- Kanban method = a lightweight agile method

### Lean Principles

1. empower the team
2. visualize work
3. embrace the scientific method
4. improve the "flow" of value
5. build quality in

#### Empower the Team

1. diverse skills
2. trust team members to make decisions
3. improves team satisfaction

#### Focus on Value

- eliminate waste
- continuously strive for simplicity
- don't compromise on quality

#### Visualize Work

- visual control: Toyota uses kanbans to signal and control the work
- andon board: an information board that shows any existing problems

#### Map the Value Stream

1. draw the current state
2. draw the desired future state
3. iterate toward the future state

#### Types of Waste

- overproduction
- waiting
- transportation
- processing
- inventory
- movement
- making defective products

#### Experiment using the scientific method

1. continuously learn and improve
2. embrace change

#### Improve the "flow" of value

1. improve the whole system
2. map the value stream
3. limit work in progress
4. pull work
5. eliminate waste
6. reduce setup times
7. automate what should be automated

#### Build quality in

1. the process should identify problems
2. fix problems when they are discovered
3. identify and fix the root cause

### Agile Principles

#### Agile Manifesto

- **individuals and interactions** > processes and tools
  - *empower the team*
    - select motivated individuals
    - teams should self-organize
    - collaborate to create shared understanding
- **working software** > comprehensive documentation
  - *plan, develop, and deliver incrementally*
    - prefer conversations for conveying information
    - continuously refactor to maintain agility
    - maintain a sustainable pace
    - completed work items are the primary measure of progress
- **customer collaboration** > contract negotiation
  - *partner with the customer*
- **responding to change** > following a plan
  - *embrace change*
    - partner with the customer
    - obtain fast feedback
    - continuously inspect and adapt

#### 12 Agile Principles

> 1. "Our highest priority is to satisfy the customer through early and continuous delivery of valuable software"

- plan, develop, and deliver incrementally
- obtain fast feedback
- focus on value

> 2. "Welcome changing requirements, even late in development. Agile processes harness change for the customer's competitive advantage."

- embrace change; it provides a competitive advantage

> 3. "Deliver working software frequently, from a couple of weeks to a couple of months, with a preference to the shorter timescale."

- plan, develop, and deliver incrementally... with a preference to a higher frequency
- obtain fast feedback

> 4. "Business people and developers must work together daily throughout the project."

- collaborate to create shared understanding

> 5. "Build projects around motivated individuals. Give them the environment and support they need, and trust them to get the job done."

- select motivated individuals
- empower the team

> 6. "The most efficient and effective method of conveying information to and within a development team is face-to-face conversation."

- prefer conversations for conveying information

> 7. "Working software is the primary measure of progress."

- completed work items are the primary measure of progress

> 8. "Agile processes promote sustainable development. THe sponsors, developers, and users should be able to maintain a constant pace indefinitely."

- maintain a sustainable pace; it is a byproduct of the proper use of agile processes

> 9. "Continuous attention to technical excellence and good design enhances agility."

- don't compromise on quality
- continuously refactor to maintain agility

> 10. "Simplicity--the art of maximizing the amount of work not done--is essential."

- continuously strive for simplicity
- focus on value, eliminate waste

> 11. "The best architectures, requirements, and designs emerge from self-organizing teams."

- teams should self-organize; it results in the best outcome

> 12. "At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly."

- continuously inspect and adapt

#### Lean vs. Agile

- Lean
  - used at MIT by John Krafcik (1988)
  - describes the ideas of the Toyota Production System
  - applies to any type of project
- Agile
  - used by the participants who created the Agile Manifesto (2001)
  - described a lightweight alternative to waterfall software development
  - applies to any type of project
- terms are often used interchangeably

## Week 3

### Quick Search and Basic Search

- viewing a project's progress
  - boards
  - search
  - reports
  - dashboards

- searching
  - 1. **quick search**: text search of issues, boards, projects, and filters
    - behavior is similar in cloud version and server version
    - can use `NOT` keyword to exclude terms
    - can use `OR` keyword to search for items containing one term or another
    - `AND` is assumed between keywords and doesn't do anything
  - 2. **basic search**: uses interface elements to search for issues
    - textbox
    - can select more dropdown to search other fields such as priority or date
  - 3. **advanced search (JQL)**: text-based search called JQL to search for issues
  - 4. **filters**: convenient saved searches
  - 5. **quick filters**: limit the issues that are visible on a board

- why is searching important?
  - adapt your jira experience to your team's processes

### JQL

#### Basic and Advanced JQL Search

- click on "Switch to JQL" link
- `project = PROJ order by lastViewed DESC`
  - `project = PROJ`: select a subset of issues
  - `order by created DESC`: order the results
- JQL = Jira Query Language
  - search issues only
- basic search
  - user friendly interface
  - queries can be complex
- advanced search
  - uses JQL
  - most powerful search method
  - JQL can be used in automation scripts
- "writing" JQL - the easiest way: let Jira write it for you

#### JQL with Autocomplete

- shows up to 15 matches
- syntax help points you to documentation for advanced search
- after project is entered, suggested operators show up
- then select another project
- automatically add order by clause by clicking on a column to sort by
- advanced search fields reference to learn more!

#### Order By

- simplest JQL statement: ''
- `order by key DESC` returns same results
- can search by two columns, but order of columns matters

#### Functions

- issue selection clauses
  - `<field name> <operator> <field value>`
  - `project = projectA`
  - `<field name> <operator> <function>`
  - `assignee = currentUser()`
- time functions
  - `now()`
- time unit qualifier
  - created in the last 2 days (48 hours)
    - `created > -2d`
  - created since the start of day 2 days ago
    - `created > startOfDay(-2d)`
  - created since the 15th of this month
    - `created > startOfMonth(+14d)`

#### Operator

- can use operator autocomplete by entering field name and then pressing space bar
- `=` (equals)
- `!=` (not equals)
- `>` (greater than)
- `>=` (greater than or equals)
- `<` (less than)
- `<=` (less than or equals)
- `in` and `not in` *used with comma-separated list of values in JQL*
- `is` and `is not` *can only be followed by EMPTY or NULL*
- `~` and `!~` *used with text fields only*
  - searching text fields using JQL
    - `text ~ "item NOT 2"`
- `was in` and `was not in` *issue had a field value from a set of values*
  - predicates for `was` related operators
    - `AFTER "date"`
    - `BEFORE "date"`
    - `DURING ("date1", "date2")`
    - `ON "date"`
    - `BY "username"`
  - find all issues moved to teh Done status by the current user in the past month
    - `status was Done BY currentUser() AFTER -1M`
- `changed` *issues have a field value that has changed*
  - additional predicates for `changed` operator
    - `FROM "oldvalue"`
    - `TO "newvalue"
  - example
    - `status changed FROM "Done" TO "In Progress"

#### Boolean Operators

- `AND`
  - `assignee = currentUser() AND status = "In Progress"`
- `OR`
  - `status = "Selected for Development" OR status = "In Progress"`
  - `status in ("Selected for Development", "In Progress")`
- `NOT`
  - `NOT status = Backlog`
  - `status != Backlog`

- find unresolved issues in all projects except SampleA
  - `resolution = Unresolved AND NOT project = SampleA`

- use parentheses to set Boolean operator precedence and improve query readability
  - 1. parentheses take precedence
  - 2. AND normally has precedence over OR

### Filters

- filters = saved searches which allow you to quickly execute a search
- execute search > save as > name filter
- details > access > edit permissions

#### Board Filters

- every board has a filter that defines the issues shown on the board
- can edit the board's filter
- if you create a board, you might assign it to a filter
- a board filter can be used to show issues from multiple projects on a single board

#### Quick Filters

- can be used to refine reports
