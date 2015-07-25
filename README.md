# git-components

This is a git repository structure one might want to use to manage multiple
independent components of a microservice-oriented system in a single git repository.

Each component (microservice) is managed in its own branch with its own history and its own set of release tags.

The system is composed by checking out a set of component branches whith a very simple manifest-like bash script. 

For the sake of clean organisation the following conventions should be respected:

1. There are 2 types of branches: component branches and system branches.
 - component branches contain all of the source code of a single component.
 - system branches contain a manifest-like bash script, `checkout-all.sh`
2. The following naming conventions apply to component branches:
 - `component-<component_name>` (this is the component master branch e.g. `component-1`)
 - `component-<component_name>-<branch_name>` (this is a per-component non-master branch, e.g. `component-1-experimental`)
3. The following naming conventions apply to system branches:
 - `system` (this is the system master branch, which should declare the latest version of all components in its `checkout-all.sh`)
 - `system-<branch_name>` (this is a system non-master branch e.g. `system-clientA`. It can declare a mix of the latest versions of components or component release tags or component experimental branches in its `checkout-all.sh`. It doesn't have to declare all system components.)
 4. The following naming convention applies both to system and component release tags:
 - `[system|component]-<semver>` (semver is a [semantic version number](http://semver.org/), e.g. `component-1-0.0.1`)
5. The checkout-all.sh should be a bash script containing only lines of the following format:
 - `git checkout [<component branch>|<component release tag>] -- .` (trailing dot is important)
 
## how can I use it?

1. Clone this repository and `git fetch refs/*:refs/*` to set up all branches and release tags appropriately. 
2. `git checkout system`
2. Inspect the `checkout-all.sh` to see an example manifest file with the latest versions of all components.
3. Execute `checkout-all.sh` to compose the latest system.
4. In order to prepare working on component-1 clean your index and working tree with `clean-all.sh`.
5. Checkout component-1 master branch with `git checkout component-1`
6. Do your work and commit it or branch it away or create a release tag respecting the naming conventions.
7. `git checkout system-clientB` to see a `checkout-all.sh` file declaring a release tag and a non-master branch of a component.
