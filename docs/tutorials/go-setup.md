# Setting up a dev container for Go

* Primary author: [Kishan Gajera](https://github.com/gajekish)

* Reviewer: [Dan Shallal](https://github.com/dshallal)

Welcome! In this tutorial, you'll learn how to set up a development container for Go. We'll guide you through creating a Go Development Container in Visual Studio Code, enabling a consistent and efficient development environment.

### What You Will Learn
After finishing this tutorial, you will:

* Set up a basic Go Development Container in VS Code to streamline development.
* Initialize and configure a GitHub repository for your Go project.
* Run a simple "Hello COMP423" program in Go within the development container.
* Gain practical experience with tools and practices commonly used in Go development and open-source projects.

## **Prerequisites**

Before we dive in, make sure you have:

1. **A GitHub account:** If you don’t have one yet, sign up at [GitHub](github.com).
2. **Git installed:** [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.
3. **Visual Studio Code (VS Code):** Download and install it from [here](https://code.visualstudio.com/).
4. **Docker installed:** Required to run the dev container. [Get Docker here](https://www.docker.com/products/docker-desktop/).

## **Part 1: Setup**

### Step 1: Create a Git Repository

1. Open your terminal or command prompt to create a new directory.
2. Create a new directory for your Go project by running:
```bash
mkdir go-dev-container
cd go-dev-container 
```
3. Set up a new Git repository
    ```bash 
    git init
    ```
4. Create a README.md file that will serve as an initial commit:
```bash
echo "# Dev Container for Go" > README.md
git add README.md
git commit -m "Initial commit with README"
```
Now that the Git repository is set up, let’s connect it to a remote hosting platform. In this tutorial, we’ll be using GitHub.
### Step 2: Create a Remote Repository on GitHub
1. Log in to your GitHub account and go to the [Create a New Repository](https://github.com/new) section.
2. Name this new repository ```go-dev-container```
3. Add whatever description you want and have the visibility set to public.
4. Make sure to **not** to initialize the repository with a README, .gitignore, or license.
5. Lastly, click **Create Repository** 
### Step 3: Link your Local Repository to GitHub
1. Add the GitHub repository as a remote:
```bash
git remote add origin https://github.com/<your-username>/go-dev-container.git
```
Replace ```<your-username>``` with your GitHub username.
2. Check the name of your default branch using the command ```git branch```. If it’s not named ```main```, you can rename it to ```main``` with the following command: ```git branch -M main```.
3. Push your local commits to the GitHub repository:
```bash
git push --set-upstream origin main
```

    !!! note "What does the --set-upstream flag actually do?"

        The command ```git push --set-upstream origin main``` pushes the local main branch to the remote repository named origin. The ```--set-upstream``` flag establishes a tracking relationship between the local main branch and the remote branch. Once set, future ```git push``` and ```git pull``` commands can be executed without explicitly specifying the branch name, simplifying workflows. The ```--set-upstream``` flag can also be written as ```-u``` for brevity.

4. Go back to your web browser and refresh your GitHub repository. You should now see the commit you made locally has been *pushed* to the remote. To verify, you can run ```git log``` locally to view the commit ID and message, which should match the most recent commit on GitHub. This confirms that your changes have been successfully pushed to the remote repository.

## **Part 2: Setting up the Development Environment**

### What is a Dev (Development) Container?

A dev container provides a consistent development environment that works seamlessly across different machines. Essentially, a dev container is a predefined setup, typically powered by Docker, that creates isolated and reliable environments for development. Think of it as a self-contained "mini computer" within your system, preloaded with everything required for a specific project.

Why is this important? In the tech industry, projects often rely on a precise set of tools and dependencies to function correctly. Without a dev container, developers must configure their environments manually, which can lead to errors, inconsistencies, and wasted time. A dev container ensures that everyone on the team works in an identical environment. It also streamlines the onboarding process for new team members, allowing them to get started quickly with minimal setup.

### Step 1: Add Development Container Configuration
1. Open the ```go-dev-container``` directory in VS Code. Navigate to File > Open Folder and select the ```go-dev-container``` directory.
2. Install the Dev Containers extension in VS Code: Open the Extensions view (```Ctrl+Shift+X``` or ```Cmd+Shift+X``` on macOS), search for "Dev Containers," and click Install.
3. Create a ```.devcontainer``` directory at the root of your project and then set up the following configuration file inside this directory: 
```bash
.devcontainer/devcontainer.json
```

The ```devcontainer.json``` file defines the configuration for your development environment. Here, we're specifying the following:

* ```name```: A descriptive name for your dev container.
* ```image```: The Docker image to use, in this case, the latest version of a Go environment.
* ```customizations```: Adds useful configurations to VS Code, like installing the Go extension. When you search for VSCode extensions on the marketplace, you will find the string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.
* ```postCreateCommand```: This specifies a command to execute after the development container is created. In this instance, it initializes a new Go module and tidies up dependencies by running ```go mod init``` and ```go mod tidy```.
```json
{
    "name": "Go Dev Container",
    "image": "mcr.microsoft.com/vscode/devcontainers/go:latest",
    "customizations": {
    "vscode": {
        "settings": {},
        "extensions": ["golang.go"]
        }
    },
    "postCreateCommand": "go mod init go-dev-container && go mod tidy"
}
```

!!! Note "golang.go VS Code Extension"
    The Go extension for VS Code by Google makes Go development easier with features like syntax highlighting, code completion, debugging, linting, formatting, and test support. It also manages Go modules and provides live error checks for a smooth coding experience.
### Step 2: Reopen the Project in a VSCode Dev Container
Reopen the project in the container by pressing ```Ctrl+Shift+P``` (or ```Cmd+Shift+P``` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab, open a new terminal pane within VSCode, and try running ```go version``` to check which version of Go was installed. You should see a recent version of Go was installed and that your dev container was properly created.

## **Part 3: Writing and Running a Go Program**

In the previous part, we used the ```postCreateCommand``` in our ```devcontainer.json``` file to initialize a new Go module in order to run a program. If this was not present in your ```go-dev-container```, you can run the following command:
```bash
go mod init go-dev-container
```

### Step 1: Make a New Go File
Make a new file in your ```go-dev-container``` directory and name it ```main.go```.
### Step 2: Write your First Go Program
In the ```main.go``` file, write the following code to create a basic program that prints "Hello COMP423."
```bash
package main

import "fmt"

func main() {
    fmt.Println("Hello COMP423")
}
```
### Step 3: Run your Newly Written Program
Open a new terminal in VSCode and run the following command:
```bash
go run main.go
```
Alternatively, you could run the following series of commands that will do the same:
```bash
go build
./go-dev-container
```
After running those commands, you should see your terminal output "Hello COMP423".

!!! abstract "What is the difference between these methods to run a program in Go?"
    When using the ```go run``` command, your code is first compiled into an executable and then executed immediately. On the other hand, the ```go build``` command works similarly to how ```gcc``` operates in C. For instance, when running ```gcc main.c```, the compiler generates an executable file with the same name as the directory. It can be executed directly by running ```./<directory_name>```.
## **Part 4: Push your Changes to a Remote Repository**
Now that we've made updates to our project, it's important to commit and push the changes to the remote repository we made earlier in the tutorial to ensure it stays up to date.
Run the following commands in your terminal to move any new changes to the remote repository:
```bash
git add .
git commit -m "Hello COMP423 program in Go"
git push
```
## **Congratulations!**
Good work on successfully following this tutorial and creating your first program in Go using a dev container. These skills are invaluable for streamlining development workflows, ensuring consistent environments, and collaborating effectively on projects.

**Works Cited**: This tutorial takes inspiration from Kris Jordan's [Starting a Static Website Project with MkDocs](https://comp423-25s.github.io/resources/MkDocs/tutorial/). Specifically in regards to the following sections: Prerequisites, Part 1, and Part 2.