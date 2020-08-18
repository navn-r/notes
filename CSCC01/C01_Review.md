---
slug: "final-review"
title: "Final Review"
---
# CSCC01 - Final Review
#### Fall 2020    
> The fix from StackOverflow isn't working
  
## Software Development - The Agile Mindset 
> Iterative approach to software development 

### Values of the Agile Manifesto:

- **Individuals** **and** **interactions** over processes and tools
- **Working** **software** over comprehensive documentation
- **Customer** **collaboration** over contract negotiation
- **Responding** **to** **change** over following a plan

### User Stories
> Short, simple descriptions for features as told by the end-user

Commonly follows the template:
```html
As a <User>, I want (to) <Goal>, (so that)/(in order to) <Value> 
```
- Never more than 2 brief sentences, but usually one

- Template should not mention any technical detail/requirement, only end-user related details

- Often written on index cards, but can be referenced on a Sprint/Kanban board

- All three attributes (`User, Goal, Value`) **_must_** be mentioned in a user story

- To further add detail to a User Story, mention a clear Acceptance Criteria or if need be, break it down into smaller stories/sub-tasks

- What makes a good user story? (**INVEST**)
    - Independent
    - Negotiable
    - Valuable
    - Estimatable
    - Sized Appropriately
    - Testable

### Burn Down Chart
> Tool for collecting sprint/project data  
> https://www.projectmanager.com/blog/burndown-chart-what-is-it

- Used for viewing how much **_work_** is left w/r/t how much **_time_** is left

- Usually, x-axis is measured in days/weeks, and y-axis is measured in story points

- The 'Ideal Work Remaining Line' is a straight line connecting the start of the sprint to the end linearly

- The 'Actual Work Remaining Line' is the *actual* work that has been done throughout the sprint, and hence deviates above or below from the ideal

- Having a visual represenation of progressional data keeps the team more informed and on the same page

- Doesn't show whether the team is working on the **_right_** things

- Depending on how well of an estimate user stories are story pointed, this chart may not accurately show if a team is on track or not

### Critical Path
> The sequence of activities that will take the longest to complete, determining the overall length of the project  
> https://www.mountaingoatsoftware.com/blog/the-critical-path-on-agile-projects

- Can be answered 2 ways:
  - What is the CP within an Iteration?
  - What is the CP within a project?

- CP can be quickly identified when certain user stories lead/build upon the next/previous one

- Easy to spot due to a short iteration length

- Must be considered by the team, but does not have to be very formal

- Useful when you need to consider future dependencies along the line that may affect what is currently being developed

### Agile Frameworks

- **Scrum** 
    > Set of meetings, tools, and roles that work in concert to help teams structure and manage their work.   
    > https://www.atlassian.com/agile/scrum
    - Centered around continuous improvement and adjustment

    - Structured to help teams natually respond and adapt to change in a **_timely manner_**

    - Scrum Master *dedicated* to co-ordinating the scrum and resolving blockers

    - Sprints:
      - Short, **_time-boxed_** period of time where the scrum works to complete a *set amount of work*
      - Common sprint length is 2 weeks, but can range from 1 week to 1 month

    - Scrum Meetings:
      - Sprint Planning Meeting:
        - Determine the User and Technical Stories that will be worked on during the current sprint
        - Sprint Estimates (Story Pointing) may or may not occur during this meeting

      - Daily Standup: 
        - Brief meeting involving the whole scrum, highlighting progress and identifying blockers
        - Must be done at the same time every day, preferably at the same location
        - Usually done standing up, but should reference the Scrum Board
        - *No 'code-talk' during this meeting*  
        - Participants must answer the following:    
          1. What have you done since yesterday?    
          2. What are you planning on doing today?  
          3. Any impediments or stumbling blocks?  

      - Backlog Refinement:
        - Story Point items found in the backlog 
        - Common pointing schema is Fibonacci (1, 3, 5, 8, 13,...)
        - Story Points represent the **_relative required amount of effort_**
        - Must refer to Definition of Done (DoD) and *collective* effort (Dev + QA)

      - Sprint Retrospective:
        - Meeting to reflect on the completed sprint
        - Must ask what went well, and what did not
        - Solutions to fix mistakes are proposed for the next sprint 

- **Kanban** 
    > Real time communication of capacity and full time transparency of work  
    > https://www.atlassian.com/agile/kanban

    - Work items are represented on a central Kanban Board
    - Board should be seen as the 'single source of truth' for the team's work
    - Entire team's responsiblity to ensure work is moving smoothly through the board
    - Each work item is represented by a single Kanban Card and is positioned in swimlanes based on the state of completion
    - Example swimlanes include `To Do, In Progress, In Review, Waiting on QA, Done`

- Test Driven Development (**TDD**) 
    > Develop tests before writing the code.

    - The tests become sort of like the 'acceptance criteria' or specification for development

    - Traditionally, TDD means:
      - Write failing test cases
      - Write the minimum amount of code to pass the test(s)
      - Repeat with occasional refactoring

    - Agile teams can adopt TDD with different types of testing:
      - Unit Testing
      - Integration Testing
      - Product Acceptance Criteria (Customer Requirement) Testing
      - Regression Tesing


- Extreme Programming (**XP**) 
    > Iterative Incremental model incorperating TDD
    
    - Customer's decisions drive the product
    - Development team works *directly* with Product Owner or Domain Expert
    - Focused on delivering working software rather than documentation


## Software Design
> It's a design choice

#### Coupling
> How much a class is directly linked to another class

  - High coupling between classes means that changes to one class may lead to changes in the other coupled classes

  - **_Low_** coupling is desired

#### Cohesion
> How much the features of a class belong together

  - Low cohesion means that methods in a class operate on unrelated tasks. This means the class does jobs that are unrelated

  - **_High_** cohesion means that the methods have strongly-related functionaly.

#### Dependency Injection
> Seperate the responsibility of resolving object dependency from its behaviour

- It is an Enterprise Design Pattern: used in enterprise applications

- The Injector module is basically a container, and owns the life cycle of all objects (classes) defined/instantiated under its scope

- Many different ways to implement dependency injection (ex. `Dagger2`, `Angular`)

- Needs to be instructed/configured to signal that a class has certain dependencies and how to resolve them
<br>

**Constructor Injection** (`Java`)

```java
public class MarkBooster {
    private CheatSystem cheater;

    @Inject // signals injector that MarkBooster has CheatSystem as dependency
    public MarkBooster(CheatSystem cheater) {
        this.cheater = cheater;
    }
}
```

**Setter Injection** (`Java`)

```java
public class MarkBooster {
    private CheatSystem cheater;

    // default constructor
    public MarkBooster() {}

    @Inject // resolves CheatSystem dependency
    public void setCheater(CheatSystem cheater) {
        this.cheater = cheater;
    }
}
```

### SOLID principles of design

#### Single responsiblilty principle: 

  - A class should have **_one and only one_** reason to change

  - The responsibility should be *encapsulated* by the class

  - All services for that class should be aligned with that responsibility


#### Open/closed principle: 

  - Open: Available for Extension

  - Closed: Available for use by other modules

  - Classes should be **_open_** for extension but **_closed_** for modification

  - Add new features by *extending* the class, which may or may not have the same interface(s) as the original class

#### Liskov substitution principle: 

  - Subclasses should **_add_** to a base class's behavior, not replace it
  
  - If `S` is a subtype of `T`, then objects of type `S` may be subbed for objects of type `T` without altering any of the desired properties of the program

#### Interface segregation principle: 

  - No client should be forced to *depend on methods it does not use*

  - Many client-specific interfaces are better than one general-purpose interface

  - Easier to extend and modify the design

#### Dependency inversion principle: 

  - High-level code **_shouldn't depend_** on low-level code. Both should depend on **abstractions**. It addition, abstractions shouldn't depend on details

  - Develop high level classes first, then develop lower-level classes

  - Details depend on abstractions, not concretions