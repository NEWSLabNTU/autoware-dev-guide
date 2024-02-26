# Start an Autoware Project

## Start a Git Repository

Let's create a new Git repository to start our project.

```bash
mkdir my-project
cd my-project
git init
```

Download and copy the [Makefile](../media/Makefile) to the repository. It
provides the these commands

```bash
make prepare  # Install dependencies
make build    # Build the project
make clean    # Clean up compilation output
```

Let's import Autoware packages to the `src/autoware` directory. It
downloads the
[autoware.repos](https://raw.githubusercontent.com/autowarefoundation/autoware/main/autoware.repos)
file that contains the list of Autoware packages. The the command
`vcs2git` imports these packages as Git submodules. Git submodule is a
nice feature to record the commit hashes and remote repository
links. The merit of this step is to help us pin down package versions.


```bash
wget https://raw.githubusercontent.com/autowarefoundation/autoware/main/autoware.repos
vcs2git autoware.repos src
git commit -m 'Import Autoware packages as submodules'
```

At this point, the project layout looks like this.

```
my-project
├── Makefile
└── src
    └── autoware
        ├── core
        ├── launcher
        ├── param
        ├── sensor_component
        ├── sensor_kit
        ├── src
        ├── universe
        └── vehicle
```

