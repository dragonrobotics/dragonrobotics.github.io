# Operations Manual

## Development Environment
Your development environment should have the following programs / packages installed:
 * [Atom](https://atom.io), or an equivalent text editor or IDE.
 * [git](https://git-scm.com)

Most of these instructions assume you have ready access to a shell with git installed.
If you're using Windows, the Git Bash shell is good for general work, though `cmd.exe` is a workable alternative.

## Git

### Configuration
Before you commit anything, ensure Git is configured with your name and email address:
```shell
$ git config --global user.name "[real name]"
$ git config --global user.email "[email address associated with Github account]"
```

### Committing code
To see what files are staged and modified:
```shell
$ git status
```

To add and remove files to your next commit:
```shell
$ git add path/to/file  # to add files to staging
$ git rm path/to/file  # to delete files in Git
$ git reset HEAD path/to/file  # to unstage files
$ git checkout -- path/to/file # to reset files to last commit
```
These commands are also listed when you run `git status`.

To actually make a commit:
```shell
$ git commit
```

This will (by default) drop you into the Vim editor to edit a commit message.
Press `i` to start typing in your message, and hit `ESC` and then type `:wq` to save and exit.

### Branching
To create a new branch:
```shell
$ git branch <branchname>
```

To delete a branch:
```shell
$ git branch -d <branchname>
```

To switch branches:
```shell
$ git checkout <branchname>
```

To merge another branch into the current branch:
```shell
$ git merge <other-branch>
```

### Remote Operations (Push / Pull)

In both of these commands, `<remote>` refers to a named _remote repository_: this is usually a repository on Github.

For example, `origin` refers to the repository you cloned from initially. This should be your fork on Github.
You should also set up a remote pointing to the team's main repository, using this command:
```shell
$ git remote add upstream <url>
```
`url` will be specific to the repository you are working on.

To get new commits from a remote repository:
```shell
$ git pull <remote> <branch>
```

To publish your commits to a remote repository:
```shell
$ git push <remote> <branch>
```

## Robot Operations

In all RobotPy projects, the `robot.py` file serves as the main entry point.

### Testing
To run automated tests for robot code:
```shell
$ python3 robot.py test
```

### Deployment
To deploy code to the robot:
 1. Ensure you are connected to the robot-internal network.
    This usually means you should be connected to the robot via Ethernet.
 2. Run the following command:
    ```shell
    $ python3 robot.py deploy
    ```
    
Code deployment usually takes a few minutes to complete. Note that all tests are run before deployment;
if any of these fail, you should NOT deploy the code to the robot.

### Robot Preferences
Robot preferences and configuration can be set easily by using the SmartDashboard.
You may need to add the 'Robot Preferences' widget to the dashboard; You can do this by selecting
`View > Add > Robot Preferences` in the SmartDashboard toolbar.


## Troubleshooting

### Deployment: SSH fails to connect (connection refused)
Code deployment to the robot should be done over an Ethernet connection.
For some reason, the deployment process may be blocked by the radio if done over Wi-Fi.

### Deployment: my code doesn't seem to be updating when I deploy it
The robot may be using stale .pyc files instead of the latest deployed code.
Try sshing into the robot (`ssh admin@roborio-5002-frc.local`) and deleting the .pyc files manually.
