# **1. 🛠️ You don’t need to be a DevOps engineer, but you do need to understand your tools. That’s what this session is for.**

![[Pasted image 20250603094245.png]]
**"There is no truth in flesh, only betrayal."**  
– Every systems engineer after debugging a manually cobbled-together server with five undocumented config changes and a Bash script no one’s touched since 2019.
## **1.1 🎯 Purpose of the Session (1–2 min)**

**"Today we’re getting hands-on with Git, Bash, and Ansible three of the most useful tools in the daily life of a developer, sysadmin, or any vaguely techy creature trying to survive in production."**

This session is designed for:

- Curious technomancers, analysts, and technical team members who occasionally work on Linux systems or automation projects
    
- Folks who want to stop relying on copy-paste scripts and start understanding what the code actually _does_
    
- Anyone who wants to debug a broken shell script or tweak an Ansible playbook without panic
    
- People who are curious about automation but don’t know where to start
    

This isn’t end all be all automation/IaaC training. It’s the foundation the “aha” moment where things start to make sense.

> The real goal: give you enough mental models, patterns, and structure that you can be given a branch, explore Bash scripting or Ansible without getting lost or overwhelmed.

## **1.2 🤖 Why These Tools Matter (2–3 min)**

In most modern technical teams, these three tools come up constantly whether you're in development, QA, infrastructure, systems engineering or just building well engineered tech stuff.

- **Git** is how teams track changes, collaborate on code, and recover from mistakes. It’s more than `git push` it’s a safety net, and a time machine.
    
- **Bash** is the command-line language of Unix systems. Learning Bash basics gives you the power to automate tasks, understand existing scripts, and fix small annoyances yourself.
    
- **Ansible** is a tool for describing and applying changes to systems through simple, readable code. Because SSH-ing into 30 servers to run the same command manually is not a personality trait
    

Understanding _how_ these tools work not just _what_ they do makes you more effective, more self-sufficient, and more valuable team.

## **1.3 💡 What You’ll Leave With (1–2 min)**

By the end of this session, you’ll be able to:

- Understand and use basic Git commands for version control and collaboration
    
- Write simple a Bash script with conditionals and functions
    
- Read and debug Ansible roles, and even create a basic one from scratch
    
- Grasp the _structure_ behind each tool so you can learn more as needed, not feel lost when reading documentation or Stack Overflow answers
    

We’re not chasing mastery today. We’re laying down the kind of understanding that helps you keep learning on your own, collaborate without guessing, and build your confidence over time.


# **2. 🧬 Git Basics   Learn to Time Travel (Safely)**

![[Pasted image 20250603095947.png]]

"You and your senior dev if Git didn’t exist and you just overwrote `main` with a ZIP file named `final_final_reallydone2.zip`."

> _“Version control is like saving your game except if you mess up and don’t know how to revert, you’re stuck fighting a boss with one HP and no potions.”_

Git isn’t just a tool it’s a _habit._ It’s how teams work together without stepping on each other’s toes (much). It’s how you try ideas, fix bugs, roll back mistakes, and prove you didn’t break prod.

## **2.1 🌳 What Even _Is_ a Git Repository?**

Before we touch a command, let’s ground this:

A **Git repository** is a system for tracking changes to files over time. It lets you work on a project, save your progress, branch off into new ideas, undo mistakes, and collaborate without chaos.

But technical definitions are boring. So let’s make it visual:

---

### 🧠 **Imagine your codebase as a tree.**

- The **main branch** is the **trunk** – the clean, central line of code development. It’s where you eventually want your polished, working code to live. 
    
- **Branches** grow off the trunk. These are used for features, fixes, or experiments. Working on a branch means you’re isolated safe to break things without hurting the trunk.
    
- Each **commit** is a ring in the tree’s wood a snapshot of your project at that moment in time.
    

But here’s the catch:

> **Git isn’t a normal tree.** In Git, branches can:
> 
> 	- Outgrow the trunk
> 	    
> 	- Merge into each other
> 	    
> 	- Loop around in strange directions
> 	    
> 	- Get abandoned and rot quietly in the dark...
>     

![[Pasted image 20250603104103.png]]
This is _great_ for experimentation but without structure, it turns into an unholy monument to some eldrich tree god. 


---

### 🔥 Without a maintainer, the tree becomes chaos:

If nobody is reviewing changes, deleting dead branches, or keeping the trunk strong, you get:

- Orphaned branches no one remembers
    
- Duplicate work across timelines
    
- Merge conflicts from 3-week-old features
    
- Bugs reappearing from merged zombie code
    

---

### 📋 So who’s responsible for keeping order?

|**Role**|**What They Do in Git**|
|---|---|
|**Developer**|Creates branches, writes code, commits changes. They explore the tree.|
|**Maintainer**|Reviews, merges, prunes branches. They shape the tree and guard the trunk.|
|**Owner/Admin**|Sets permissions, manages the repo’s visibility and policies. They protect the forest.|
#### In practice:

- You as a **developer** create a branch (`feature/login`), make commits, and push your work.
	- The main branch is protected for only the maintainers to merge code into. Not mere devs can just push their code straight to main branch.
    
- The **maintainer** reviews your pull request, makes sure it won’t break things, and merges it into `main`.
    
- The **owner** might require reviews, enable branch protection, or enforce commit signing.
    

---

### ✅ Why does this matter?

Git isn’t just a backup tool. It’s a collaboration tool.

When used well, Git helps you:

- Work without stepping on each other’s code
    
- Roll back when something breaks
    
- Explore ideas safely
    
- Understand how your project evolved
    

> Without Git? Every change is permanent. Every mistake is a crisis. Collaboration is a gamble.  
> **With Git?** Time travel. Parallel worlds. A full record of every experiment. And, if needed... the power to undo a disaster.

---

## **2.2 🌿 Branching & Merging – Playing with Timelines Without Breaking the Universe**

Now that you know Git is a magic time tree with cursed branching potential, let’s learn how to use that power _without accidentally creating a paradox_.

### 🧪 Branching: Your Personal Side Quest

In Git, a **branch** is just a pointer to a specific commit in the timeline. When you make a new branch, you're creating a safe space to:

- Build a feature
    
- Fix a bug
    
- Try something weird and potentially stupid (we support this)
    
- Work without affecting the mainline code
    

#### 🚀 Create a new branch:
```bash
git branch feature/login
```
This creates the branch, but doesn’t switch to it yet.

#### ✨ Switch to a branch:
```bash
git checkout feature/login
```

or the newer (and clearer):
```bash
git switch feature/login
```
ow you’re living in your own timeline. Changes you commit here stay isolated until you choose to merge.

---

### 🔄 Merging: Combining Timelines

Once your branch has something good on it (or at least functional), you want to merge it back into `main` (or whatever your main branch is called).

#### 📦 Merge it back:
```bash
git checkout staging
git merge feature/login
```
Git tries to combine the changes. If the changes don't conflict great! You get a fast-forward or clean merge.

If there **are** conflicts (e.g., two branches touched the same line in the same file), Git will stop and ask you to fix them manually.

> **Conflicts are not errors.** They’re just Git saying:  
> _“Hey, I can’t decide which version of this file is right. You tell me.”_

---

### 🧠 Branching Best Practices (That Save Lives)

| ✅ Do This                                        | ❌ Don’t Do This                                   |
| ------------------------------------------------ | ------------------------------------------------- |
| Make a new branch for each feature or fix        | Work on `main` like it’s your personal dev branch |
| Pull from `main` often to keep your branch fresh | Let your branch drift for weeks until it mutates  |
| Delete old branches after merging                | Keep 47 half-finished branches from 2022          |
### 🔁 Bonus: Shortcuts

- Create and switch in one line:

```bash
git checkout -b feature/awesome-idea
```
Delete a branch (after merging):
```bash
git branch -d feature/awesome-idea
```
Git lets you create branches fast, merge them cleanly, and jump between timelines like a versioned time wizard.  
Just don’t forget: _clean branches are a kindness to your future self and your teammates._

## **2.3 🔄 Syncing Changes – The Real Git Flow**

Let’s walk through what you _actually_ do on a normal day when using Git. This is the “daily driver” flow. You’ll use this constantly when working on a branch.

---

### ✅ Step 1: Pull the latest changes
```bash
git pull
```
> Before you start coding, always sync with the remote so you’re working on the most recent version of the project. This prevents conflicts _before_ they happen.

---

### 🔍 Step 2: Check what changed
```bash
git status
```
This tells you:

- Which files were changed
    
- Which ones are staged (ready to commit)
    
- Which ones are untracked
    
- What branch you’re on
    
- If your branch is ahead or behind the remote
    

---

### ➕ Step 3: Add the files you want to commit
```bash
git add path/to/file
```
You can add multiple files, or add everything that changed:
```bash
git add .
```
⚠️ `git add .` adds everything...double check with `git status` before committing to avoid surprises.

🧾 Step 4: Commit your changes
```bash
git commit -m "Add new login form validation"
```
A good commit message should be short, specific, and written in the present tense (like a journal entry for your code).

### 🚀 Step 5: Push your changes
```bash
git push
```

### 🔁 Summary Flow:
```bash
git pull      # Sync with latest remote changes
git status                 # See what changed
git add path/to/file       # Stage files to commit
git commit -m "Describe your change"
git push   # Share your changes with the team
```

💡 This is the everyday Git rhythm. If you can do this confidently, you can collaborate on nearly any software project.


## **2.4 🧯 Undoing & Restoring – Git Has Your Back (Usually)**

One of the best things about Git: **You can mess up and not die.**  
Git gives you multiple ways to go back, undo, reset, or clean up mistakes if you know the tools.

This section is about **recovering** when:

- You committed the wrong thing
    
- You deleted something by accident
    
- You broke the code and want to go back
    
- You committed to the wrong branch
    
- You just want to reset the timeline and pretend it never happened
    

---

### 🔁 `git restore` – Take back local changes

If you've modified a file but haven’t staged or committed it yet, you can undo the change:

```bash
git restore path/to/file
```

> This puts the file back to its last committed state.  
> ⚠️ You’ll **lose** any unsaved work in that file.

---

### ⏮️ `git reset` – Move the branch pointer

Used to “rewind” your commits. There are three main types:

```bash
git reset --soft HEAD~1
```

- Moves HEAD back one commit, but **keeps your changes staged**
    

```bash
git reset --mixed HEAD~1
```

- Unstages changes but **keeps the code**
    

```bash
git reset --hard HEAD~1
```

- Rewinds and **deletes your changes**
    

> ⚠️ This is the nuclear option. Only use it if you _really_ want to wipe your recent work.

---

### 🪃 `git revert` – Undo a commit safely

```bash
git revert <commit-hash>
```

Instead of deleting history, `revert` creates a new commit that cancels out a previous one.  
This is the preferred way to “undo” something in a shared repo where others are relying on the timeline.

> 🧠 Use `revert` for **collaborative projects**  
> Use `reset` only if you haven’t pushed yet and you're fixing your own mess

---

### 😬 Common Mistake Scenarios

| Problem                                     | Command to Help                                            |
| ------------------------------------------- | ---------------------------------------------------------- |
| Committed to wrong branch                   | `git checkout correct-branch` → `git cherry-pick <commit>` |
| Deleted code accidentally                   | `git checkout <branch> -- path/to/file` or `git restore`   |
| Pushed a commit you regret                  | `git revert <commit>`                                      |
| Need to clean up commit history before push | `git rebase -i HEAD~N` (advanced, optional)                |
| Messed up bad and want to reset your branch | `git reset --hard origin/branch-name`                      |

---

> 🔒 Tip: **Don’t panic.** Git rarely deletes anything immediately. Most “oops” moments are fixable especially if you haven't pushed yet.


## **2.5 🧪 Hands-On Practice – Time Samurai Edition**

Alright. You’ve read enough. Let’s actually do the thing.

We're going to walk you through **making your own Git project on GitHub**, and give you some tasks to try locally so you can build muscle memory and _actually feel_ how Git works.

> ⚔️ Your mission:  
> Write a short story about a **Time Samurai** a noble warrior who fixes timelines (just like Git does). You’ll create the story file, make changes on branches, merge new chapters, and undo mistakes as you go.

---

### ✅ Step 1: Make a GitHub Repo

1. Go to [github.com](https://github.com)
    
2. Click **New repository**
    
3. Name it something like `time-samurai`
    
4. Make it **public** or **private** (doesn’t matter)
    
5. **Do not initialize with README** (we’ll do that ourselves)
    
6. Click **Create repository**
    

---

### 💻 Step 2: Clone the Repo Locally

Open your terminal:

```bash
git clone https://github.com/YOUR_USERNAME/time-samurai.git
cd time-samurai
```

Now you have a local copy of your remote repo.

---

### ✍️ Step 3: Start the Story

Create a file:

```bash
touch story.txt
```

Open it and write something like:

```
Chapter 1: The Clock Tower Falls

In the year 3024, when timelines shattered like glass...
```

Then commit it:

```bash
git add story.txt
git commit -m "Start Time Samurai story"
git push origin main
```

---

### 🌿 Step 4: Create a Branch and Write Chapter 2

```bash
git checkout -b chapter-2
```

Add to the file:

```
Chapter 2: Return to Edo

The samurai activated his chronosteel blade and stepped back into 1587.
```

Then commit and push:

```bash
git add story.txt
git commit -m "Write Chapter 2"
git push origin chapter-2
```

---

### 🔁 Step 5: Merge It Back In

Go to GitHub. You’ll see a banner suggesting you make a Pull Request. Do it.

- Click **Open pull request**
    
- Add a description
    
- Click **Merge pull request** → **Confirm merge**
    
- (Optionally delete the branch)
    

---

### 🧯 Step 6: Make a Mistake, Then Fix It

Add something wrong:

```
Chapter 3: The Samurai Eats a USB Drive
```

Commit it:

```bash
git commit -am "Add weird chapter"
```

Now undo that:

```bash
git revert HEAD
git push
```

Check the repo history. See how Git added a commit that “reversed” the mistake?

---

### 🎯 Stretch Challenges

- Try `git log` and `git diff` to explore history
    
- Try `git restore` to undo local changes
    
- Try making a new branch and editing a _different file_
    
- Try collaborating with a friend both clone the same repo and practice working on different branches
    

---

> The goal here isn’t a masterpiece story.  
> It’s building the instincts of:  
> **make a change → commit it → push it → fix it if needed.**

You’ll use this flow in every project, for the rest of your dev life.


# **3. 🖥️ Bash Scripting Basics **
![[Pasted image 20250603112546.png]]

> _“Wake the hell up, Samurai. You’ve got scripts to write.”_

We all hit the point where repeating the same terminal commands by hand starts to feel... dumb.

That’s where Bash scripting comes in.

In a world full of brittle pipelines, overengineered tools, and time pressure, you need scripts that work fast, reliably, and without second-guessing you.

**Bash is your way to automate, document, and scale your daily command-line work.** It’s not flashy but it’s powerful. Let’s build it up step by step.
---

## **3.1 🎯 Why Automate with Bash?**

Every edge counts. When you're constantly deploying quick fixes, reading logs, or rebooting half-alive systems, Bash lets you:

- Automate repetitive terminal work
    
- Chain actions into a single command
    
- Add logic to scripts (so they make decisions for you)
    
- Build small tools to solve real problems
    

> Think of it like building your own cyberdeck modules except instead of hacks, you’re scripting real-world tasks.

---

## **3.2 🧠 Building the Samurai Ops Script, Part by Part**

We’ll build a real script called `mission.sh` your cyber Merc’s command module.  
By the end, it’ll accept a mission location, confirm timeline safety, and log successful ops.

Let’s break it down.

---

### 🔹 Step 1: Variables – Identity & Loadout

```bash
#!/bin/bash

# Set character identity
handle="EdgeSamurai"
weapon="mono-katana"
```

Variables are how we store things for later. Strings, numbers, filenames it’s all fair game.

---

### 🔹 Step 2: Arguments – Input from the Operator

```bash
# $1 is the first argument passed to the script
location="$1"

echo "📍 Target location: $location"
```

This lets your script take input like:

```bash
./mission.sh ArasakaTower
```

> Bash reads that as: _“Samurai is headed to Arasaka Tower.”_

---

### 🔹 Step 3: Conditionals – Check the Timeline

```bash
if [ -z "$location" ]; then
  echo "❌ No mission target provided. Usage: ./mission.sh <location>"
  exit 1
elif [ "$location" = "NightCityDump" ]; then
  echo "⚠️ Timeline too unstable. Mission aborted."
  exit 1
fi
```

This is your logic engine: checking input, validating missions, protecting the timeline from corrupted destinations.
if -z tests if Something has ZERO LENGTH. eg. empty

---

### 🔹 Step 4: Functions – Modular Cyberware

```bash
begin_mission() {
  echo "🗡️ Deploying $handle to $location with $weapon..."
  echo "$(date): Mission launched to $location" >> ops.log
}
```

Functions let you reuse code and make scripts readable.  
Here, we log to `ops.log` so we have a record of our jumps.

---

### 🔹 Step 5: Execute the Run

Add this to the bottom:

```bash
begin_mission
echo "✅ Mission started. Check ops.log for details."
```

---

## 🧪 Final Script: `mission.sh`

```bash
#!/bin/bash

# Set up character
handle="EdgeSamurai"
weapon="mono-katana"

# Grab argument
location="$1"

# Validate input
if [ -z "$location" ]; then
  echo "❌ No mission target provided. Usage: ./mission.sh <location>"
  exit 1
elif [ "$location" = "NightCityDump" ]; then
  echo "⚠️ Timeline too unstable. Mission aborted."
  exit 1
fi

# Define mission function
begin_mission() {
  echo "🗡️ Deploying $handle to $location with $weapon..."
  echo "$(date): Mission launched to $location" >> ops.log
}

# Run the mission
begin_mission
echo "✅ Mission started. Check ops.log for details."
```

---

## ▶️ How to Use It:

```bash
chmod +x mission.sh
./mission.sh ArasakaTower
```

Output:

```
📍 Target location: ArasakaTower
🗡️ Deploying EdgeSamurai to ArasakaTower with mono-katana...
✅ Mission started. Check ops.log for details.
```

---

> 🧠 You just used **variables**, **arguments**, **functions**, and **conditionals** in one working Bash script.

This is how scripting becomes real. From here, you could:

- Add interactive prompts (`read`)
    
- Use `case` statements for mission types
    
- Or email logs to your fixer/handler (yourself)

## **3.4 🛠️ Practice Task – Build a Simple Log Rotator**

Alright, the cyberpunky stuff was fun, but heres something more real.

Let’s apply what you’ve learned in a short, real-world script that many sysadmins or devs end up needing: **a log rotation helper**.

Sometimes an app just writes to the same log file endlessly. This script will back up the existing log file and start fresh **without losing data**.

---

### 🧪 What You’ll Practice

- Using variables
    
- File checks with `if`
    
- A simple function
    
- Clean feedback to the user
    

---

### ✍️ `rotate_logs.sh`

```bash
#!/bin/bash

# Set paths
LOGFILE="/var/log/testlog.log"
BACKUP="/var/log/testlog.log.bak"

# Function to rotate the log
rotate_log() {
  echo "🔁 Rotating log..."
  mv "$LOGFILE" "$BACKUP"
  echo "$(date): Log rotated" >> "$BACKUP"
  touch "$LOGFILE"
  echo "✅ Done. Old log saved as $BACKUP"
}

# Logic
if [ -f "$LOGFILE" ]; then
  rotate_log
else
  echo "⚠️  No log file found at $LOGFILE"
  echo "You can create one with: touch $LOGFILE"
fi
```

---

### ▶️ How to Use It

1. Save it:
    
    ```bash
    nano rotate_logs.sh
    ```
    
2. Make it executable:
    
    ```bash
    chmod +x rotate_logs.sh
    ```
    
3. Create a dummy log file for testing:
    
    ```bash
    echo "This is a test log entry." >> /var/log/testlog.log
    ```
    
4. Run your script:
    
    ```bash
    ./rotate_logs.sh
    ```
    

---

### 💡 What’s Happening:

|Line|What's Going On|
|---|---|
|`if [ -f "$LOGFILE" ]`|Checks if the log file exists|
|`mv "$LOGFILE" "$BACKUP"`|Moves it to a backup|
|`touch "$LOGFILE"`|Creates a fresh, empty log file|
|`$(date)`|Stamps the backup with a timestamp|

---

### 🔧 Challenge:

Try modifying the script to:

- Append a **timestamp** to the backup filename (`testlog.log.2025-06-03.bak`)
    
- Limit to only rotating if the log is over a certain size
    
- Keep a log of all rotations in a central `rotation.log`
    

---

> 🔁 This is basic but it’s _real_. This is how scripting starts: solving your own tiny problems. From here, you build up.


# **4. 🔐 Ansible Basics 
![[Pasted image 20250603115942.png]]

**"As above so below. As in the Code, So on the Server"**

> _“Infrastructure used to be manual. Hand-built systems, handcrafted config files, prayer-based deployments.”_
> 
> _But now we codify intent. You write the spell once Ansible casts it across all your machines._

This section is about using **Ansible** to automate a real-world task: **enforcing SSH key-based login**.  
But before we dive into that, let’s break down what Ansible is and how it works.

We’ll cover:

- What Ansible is
    
- How its project structure works
    
- What roles, playbooks, handlers, inventories, and templates are
    
- How variables and conditionals work
    
- Why YAML will betray you if you’re careless
    

---

## **4.1 🧱 The Structure of Ansible – What You’re Actually Writing**

Ansible automates system configuration by connecting to your servers over SSH and running **tasks** you define in simple YAML files.

Let’s look at the pieces that make that work.

---

### 📋 **Playbooks – The Main Script You Run**

A playbook is the "orchestration file" that tells Ansible:

- What servers to target
    
- What roles or tasks to run
    
- Whether to use `sudo` (called `become`)
    
- What variables to use
    

**Example:**

```yaml
- hosts: all
  become: yes
  roles:
    - ssh_key_auth
```

- `hosts: all` – apply to all hosts in your inventory
    
- `become: yes` – run commands as root
    
- `roles:` – list of roles to execute
    

---

### 🗺️ **Inventories – The List of Target Machines**

Inventories tell Ansible which machines to connect to. They can be `.ini`, `.yml`, or dynamic.

**Simple `inventory.ini`:**

```ini
[web]
192.168.1.100 ansible_user=admin ansible_ssh_private_key_file=~/.ssh/id_rsa
```

You can group servers (`[web]`, `[db]`, etc.) and assign variables like usernames or SSH keys per host.

> If you can SSH into it, Ansible can automate it.

---

### 🧠 **Facts – Info Ansible Learns About the System**

When Ansible connects to a server, it collects facts:  
These are details about the machine, like:

- `ansible_distribution` → OS name
    
- `ansible_hostname` → machine name
    
- `ansible_default_ipv4.address` → main IP address
    

You can use these in your playbooks to make logic decisions.

**To view facts:**

```bash
ansible all -i inventory.ini -m setup
```

---

### 🧩 **Roles – Organized Automation**

A **role** is a folder that bundles tasks, templates, handlers, and variables to perform a specific job (like setting up SSH, installing NGINX, etc.).

**Typical Role Structure:**

```
roles/
└── ssh_key_auth/
    ├── tasks/        ← the actual steps to run
    ├── templates/    ← config files with variables
    ├── handlers/     ← run if something changes
    ├── defaults/     ← default values for variables
    ├── vars/         ← required, fixed values
```

You run a role by listing it in your playbook.

---

### 🧾 **Templates – Config Files Powered by Variables**

Templates are config files (usually `.j2`) that include variables in `{{ double curly braces }}`.

**Example: `sshd_config.j2`:**

```ini
Port {{ ssh_port }}
PermitRootLogin {{ root_login }}
```

Then in your task:

```yaml
- name: Deploy SSH config
  template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
```

Ansible replaces `{{ ssh_port }}` with whatever value is defined and writes it to the server.

---

### 🔢 **Variables – Values You Control**

Variables make your code reusable.

You can define them:

- In the playbook (`vars:` block)
    
- Inside the role (`defaults/main.yml` or `vars/main.yml`)
    
- In host/group-specific files (`host_vars/`, `group_vars/`)
    
- Passed on the command line (`-e "var=value"`)
    

**Example from `defaults/main.yml`:**

```yaml
ssh_port: 22
root_login: no
```

Then use those in your templates, conditionals, or tasks.

---

### 🧠 `{{ variable_name }}` – How Variables Show Up

Whenever you see `{{ some_var }}` in a task or template, Ansible is replacing that with a value from one of your variable files or facts.

You can use:

- Built-in facts: `{{ ansible_hostname }}`
    
- Your own values: `{{ ssh_port }}`
    
- Conditionals: `when: ssh_port == 22`
    

> Always quote your variables when unsure. And never use `{{ }}` on the left side of a YAML key it will break.

---

### ⚙️ **Conditionals (When)** – Only Run This If...

Tasks don’t have to run every time. Use `when:` to control behavior.

```yaml
- name: Only install NGINX on Ubuntu
  apt:
    name: nginx
    state: present
  when: ansible_distribution == "Ubuntu"
```

You can use conditionals based on:

- OS
    
- Group
    
- Host
    
- Your own variables
    

---

### 📣 **Handlers – Only Run If Something Changed**

Handlers are tasks that only run when **notified** by another task.

Example:

```yaml
# tasks/main.yml
- name: Deploy config
  template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
  notify: Restart SSH

# handlers/main.yml
- name: Restart SSH
  service:
    name: sshd
    state: restarted
```

If the file changed, Ansible will “notify” the handler. If it didn’t, the handler is skipped.

---

### ⚠️ YAML Will Betray You – Be Precise

YAML files are readable but fragile.

**Rules:**

- Use **2 spaces** for each indent (never tabs)
    
- Don’t mix spaces and tabs
    
- Always align things properly
    

**Bad:**

```yaml
- name: Breaks everything
  lineinfile: path=/etc/file state=present
```

**Good:**

```yaml
- name: Works fine
  lineinfile:
    path: /etc/file
    state: present
```

Use:

```bash
ansible-playbook playbook.yml --syntax-check
```

to validate before you run.

---

### 🧠 Summary Table

|Concept|What it Does|
|---|---|
|Playbook|Main YAML file that runs your automation|
|Inventory|Lists the target servers|
|Role|Bundle of tasks/templates/handlers for a job|
|Template|Config file with variables inside `{{ }}`|
|Variable|Data you define (via defaults, vars, etc.)|
|Fact|Info Ansible gathers automatically about a system|
|Conditional|Runs a task only if a condition is true (`when`)|
|Handler|Extra task triggered _only_ if something changes|
|Become|Lets Ansible run commands as root (`sudo`)|

# **4.3 🛠️ Creating a Role to Enforce SSH Key-Based Login**

Now that you understand how Ansible is structured, let’s actually **build a role** that applies a real system setting: **disabling password login over SSH**.

Why?  
Because SSH keys are:

- More secure
    
- Easier to automate
    
- The standard in professional environments
    

---

### ✳️ What This Role Will Do

- Modify `/etc/ssh/sshd_config` to turn off password login
    
- Use a **template** with variables for flexibility
    
- Restart SSH (but only if the config changed) using a **handler**
    

This gives you:

- Real-world experience with a complete Ansible role
    
- Confidence that you can write and apply automation safely
    

---

## **Step 1: Scaffold the Role**

In your Ansible project folder, run:

```bash
ansible-galaxy init roles/ssh_key_auth
```

You now have a folder structure like this:

```
roles/
└── ssh_key_auth/
    ├── tasks/
    ├── handlers/
    ├── templates/
    ├── defaults/
    └── ...
```

---

## **Step 2: Create the Variables**

Inside `roles/ssh_key_auth/defaults/main.yml`:

```yaml
# roles/ssh_key_auth/defaults/main.yml

ssh_port: 22
root_login: no
password_auth: no
```

These can be reused across templates or tasks.

---

## **Step 3: Create the Template**

Inside `roles/ssh_key_auth/templates/sshd_config.j2`:

```jinja
Port {{ ssh_port }}
PermitRootLogin {{ root_login }}
PasswordAuthentication {{ password_auth }}
```

This is your dynamic config file. Variables are pulled from `defaults/main.yml`.

---

## **Step 4: Write the Tasks**

Open `roles/ssh_key_auth/tasks/main.yml` and define the steps:

```yaml
# roles/ssh_key_auth/tasks/main.yml

- name: Deploy SSH config with variables
  template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
    owner: root
    group: root
    mode: '0600'
    backup: yes
  notify: Restart SSH
```

- This copies the Jinja2 template to the server
    
- It sets permissions properly
    
- It creates a backup before overwriting
    
- And if the file changed, it **notifies** a handler to restart the service
    

---

## **Step 5: Add the Handler**

Open `roles/ssh_key_auth/handlers/main.yml`:

```yaml
# roles/ssh_key_auth/handlers/main.yml

- name: Restart SSH
  service:
    name: sshd
    state: restarted
```

This only runs if the template task changed something.

---

## **Step 6: Use the Role in a Playbook**

Create or update your playbook (e.g. `secure_ssh.yml`):

```yaml
- hosts: all
  become: yes
  roles:
    - ssh_key_auth
```

You’ll also need an inventory file like:

**`inventory.ini`**

```ini
[targets]
192.168.1.100 ansible_user=admin ansible_ssh_private_key_file=~/.ssh/id_rsa
```

---

## ✅ Run It

```bash
ansible-playbook -i inventory.ini secure_ssh.yml
```

What happens:

1. Ansible connects over SSH
    
2. It deploys your templated SSH config
    
3. If the config changed, SSH restarts
    
4. Your server is now using SSH keys only
    

---

> 🧠 That’s real infrastructure automation. You just built a role that enforces policy with version-controlled, reusable code.

# **4.4 🧪 Practice Challenge – Serve Yourself (Literally)**

> You’ve just built a role that modified your system config. Now it’s your turn to create something new—from scratch.  
> **You’ll deploy a webserver and build your own page, using Ansible to automate the whole process.**

---

## 💡 The Goal

Create a role called `httpd_server` that:

1. Installs a web server (Apache HTTPD)
    
2. Starts the service and ensures it runs on boot
    
3. Uses a **Jinja2 template** to deploy a custom `index.html`
    
4. Includes a greeting using a variable like `{{ visitor_name }}`
    

You’ll use what you’ve already learned—and figure out the rest by **reading documentation, testing, and debugging.**

---

## 🗃️ Suggested Role Structure

```bash
roles/
└── httpd_server/
    ├── tasks/
    │   └── main.yml
    ├── templates/
    │   └── index.html.j2
    ├── defaults/
    │   └── main.yml
```

---

## 📄 Template Example (`index.html.j2`)

```html
<!DOCTYPE html>
<html>
  <head><title>Welcome</title></head>
  <body>
    <h1>Hello, {{ visitor_name }}!</h1>
    <p>This page was deployed by Ansible.</p>
  </body>
</html>
```

**Set the variable** in `defaults/main.yml`:

```yaml
visitor_name: CyberNetrunner
```

---

## 📘 What You’ll Need to Research

Instead of copying exact commands, try to figure these things out on your own by looking up the right Ansible modules:

- How to install a package (hint: it’s not always `yum`)
    
- How to start and enable a service
    
- How to copy or template a file into `/var/www/html/`
    
- What permissions and ownership that file should have
    

> 🔍 Search like this:
> 
> ```
> ansible module install package
> ansible apache webserver setup playbook
> ansible template permission denied
> ```

---

## 🧪 How to Run It

Once your playbook includes the new role:

```bash
ansible-playbook -i inventory.ini web.yml
```

Then in your browser, go to:

```
http://<your_vm_ip>
```

And see if your message is live.

---

## 🔁 Bonus Objectives

Try expanding the Jinja2 template with:

- `{{ ansible_hostname }}`
    
- `{{ ansible_date_time.date }}`
    
- `{{ ansible_distribution }}`
    

Or run your playbook like this:

```bash
ansible-playbook web.yml -e "visitor_name=JohnnySilverhand"
```

---

## 🛠️ Real Automation Means Troubleshooting

You’re going to run into problems. That’s not a bug—it’s the path.

Learn to:

- **Read the error message** in the red Ansible output
    
- **Google the exact module and error** (don’t skim!)
    
- **Translate blog posts into Ansible** (if it says `systemctl start httpd`, you need to find the matching module)
    
- **Log into the machine** and check logs if needed:
    
    ```bash
    journalctl -xe
    cat /var/log/httpd/error_log
    journalctl -u httpd
    systemctl status httpd
    ```
    

> 💡 **The mental model matters more than memorizing modules.**  
> You’re learning to think like someone who can automate _anything_.

---
**There is no sin in a failed run.**  
Only miswritten will.  
Correct it. Re-run it.  
**Hail thyself.**
