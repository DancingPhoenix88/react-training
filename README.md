#ReactJS (Reactive programming in general) training diary
___
## Application description
It's a daily tasks management board for single person (Search *'Bullet Journal'* - section *'Weekly'* for more information).

The first thing I do in a daily routine (on the weekday) is listing tasks that I will do in that day.

And the last thing I do in that day is checking this daily tasks list, to ensure I updated status for all of them.

### Application content
Each task should have:

- **Short description** (fit 1 line)
- **Category** (like: Company A, Company B, Personal)
- **Status**:
    - *Not started*:  New task in that day (must be changed to other status next day)
    - *In progress*:  Task in progress (increase counting each day)
    - *Finished*:     Finished task
    - *Paused*:       Task is paused temporarily for several days, due to the appearance of other high priority tasks (increase counting each day)
    - *Cancelled*:    Cancelled task. I don't need to do it anymore
    - *Delayed*:      Task which is not started yet can be delayed to the next day. A task can be a really big task, so it should be able to be splitted into some sub-tasks. Max nested level is 3 (Big task → sub-task → tiny sub-task)
It's that simple.

### Application interaction
First of all, I want a full keyboard experience. It means I can perform frequent actions just by using keyboard, because I use a laptop with a f*cking touchpad.

**Basic operations**:

- **Add a new task**
    - `[Enter]`:         Add new task right below current task (same level - add next sibling)
    - `[Shift + Enter]`: Add new task right above current task (same level - add previous sibling)
- **Select a task**
    - `[Up / Down]`:     Navigate to above / below task (no matter their level)
- **Indent a task**
    - `[Tab]`:           Make current task a child of right above task (increase level - become child)
    - `[Shift + Tab]`:   Make current task a sibling of its old parent task (decrease level - become parent)
    - All children will follow its parent
- **Change task status**
    - `[Shift + Space]`: Open status drawer and select default next status:
        - *Not started* → *In progress* → *Finished* → *In progress*
        - *Paused*, *Cancelled*, *Delayed* → *In progress*
        - User can use `[Up / Down]` to select other status and use `[Enter]` to confirm
        - User can use `[Esc]` to cancel action
    - If all child tasks are marked as *Finished* → parent task is automatically marked as *Finished*
    - If any child task is marked as *In progress* → parent task is automatically marked as *In progress*
    - If a parent task is marked as *Finished* → all children task are automatically marked as *Finished*


**Some more smart operations (which can't be done on my paper notebook)**:

- **Auto count (*Not started*, *In progress*) tasks**
- **Highlight *Not started*, *In progress* tasks**, and dim other tasks for quick look up (`[Holding Shift]` for more than 0.7s)
- **Re-order task** (for moving high priority tasks to the top)
    - `[Cmd + Up / Down]`: Move current task (and its children) up / down
- **Delete task** (just for mis-typing, don't use it for canceled tasks) using shortcut `[Shift + Delete]`
    - Its children will be deleted too (→ there should be a confirm dialog for this case)
- **Undo** using `[Cmd + Z]`, redo using `[Cmd + R]`
    - Stack last 100 actions (even after re-open this application)
    - Use `[Cmd + Shift + Z]` or `[Cmd + Shift + R]` to show 'Undo & Redo' popup
        - `[Esc]` to close popup
        - Show last actions stack (with description)
        - Position of current action in stack (selected by default)
        - Navigate by `[Up / Down]` to preview Undo, Redo in action
        - `[Enter]` to confirm and close popup
- **Clone** selected tasks (to right below) using `[Cmd + D]`
- **Copy** selected tasks using `[Cmd + C]`
- **Paste** copied tasks using as children of current task `[Cmd + V]`
- **Batch edit** tasks:
    - Select adjacent tasks by `[Shift + Up / Down]` (only applied for tasks with same parent)
    - Indent selected tasks
    - Change status of selected tasks
    - Re-order selected tasks
    - Delete selected tasks
    - Clone selected tasks
    - **Adding a new task can't be batched*
    - **Copied tasks will be pasted on the first task among selected ones*
- Cannot edit lists in the past, except next case
- On new day, if there's any tasks from last avaiable day which is (*Not started*), show options before user can start a new list:
    - Compare Before & After result
    - Update status for the last time, after Accepting this result (unless using Undo feature)
    - Button to cancel all *Not started* tasks
    - Button to finish all *Not started* tasks
    - Auto count up *Delayed* tasks
- Can search & filter for tasks (case insensitive)
    - By keywords
    - By day        (`W`:`Mon`|`Tue`|`Wed`|`Thu`|`Fri`|`Sat`|`Sun`). 
      - Multiple values combination rules: OR. 
      - Typing hint available.
    - By date       (`D`:`$date`, `M`:`$month`, `Y`:`$year`). 
      - Multiple values combination rules: `{$dates OR}` AND `{$months OR}` AND `{$years OR}`.
    - By range      (`R`:`$previous_days`)`, default: 7 days. 
      - Single value only.
    - By status     (`S`:`NotStarted`|`InProgress*`|`Finished`|`Paused`|`Cancelled`|`Delayed*`). 
      - Multiple values combination rules: OR. 
      - Typing hint available (that's why I chose different first letter for each status)
      - *InProgress* & *Delayed* could be appened with number of days count
    - By category   (`C`:`...`).
      - Multiple values combination rules: OR. 
      - Typing hint available.
- Has sound/visual effect for rejected actions (like unavailable Redo)


Some fancy features might come next along the way

___
## Git & Version logs
Git structure:

- `master`:     For reference purposes (each commit here is a 'release' version -> easy for checking diff)
- `develop`:    Main branch for development process (for multiple purposes)
- `release/v`:  When ready for a new version, create a new branch from `develop`
    - After that, create a new version log to tell what changed
    - Then merge this new branch to `master` & `develop`
- `feature/x`:  Branch out from `develop`, contains a candidate feature (complex or experiment or not-decided-in-next-version-yet)

So, each version will have a separate log (in .MD format)
