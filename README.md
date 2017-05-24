# common-flows
A repository containing the playbooks/roles for common tasks performed on existing clusters, ensembles, and groups of nodes once they have been provisioned. This includes tasks like:

* adding new nodes to existing clusters or ensembles
* deploying agents to new nodes but leaving the existing nodes untouched, and
* reconfiguring existing nodes/clusters/ensembles once they have been provisioned

This repository is used as a submodule by the main DataNexus repositories to provide access to these common flows from any of those repositories.

# Use
To use these common roles in another GitHub repository, simply add this repository as a (git) submodule of that repository.  This can be accomplished by running a command that looks something like the following:

```bash
$ git submodule add https://github.com/Datanexus/common-flows common-flows
```

This will create a `common-flows` subdirectory, then clone this repository into that `common-flows` subdirecory and checkout the latest commit from this repository, and create a `.gitmodules` file that looks something like the following:

```yaml
[submodule "common-roles"]
    path = common-flows
    url = https://github.com/Datanexus/common-flows
```

Once the `common-roles` repository has been added as a submodule, the next step is to add the `common-roles` directory to the `roles_path` used by Ansible.  This can be accomplished by creating an `ansible.cfg` file in the same directory as the playbook that will be run that looks something like this:

```yaml
[defaults]
roles_path = ../:common-roles:common-flows
```

With the submodule added and the `roles_path` modified to ensure that the roles in this repository are included in the `roles_path` used by the `ansible-playbook` command, the roles in this repository can then be used in any playbooks that might be defined in the repository that this `common-roles` repository is being added to as a submodule.

To commit the changes made by the process of adding this repository as a submodule, you would then run a series of commands that look something like this:

```bash
$ git add common-flows ansible.cfg
    ...
$ git commit -m "adding common-flows submodule to repository"
    ...
$ git push
    ...
```

# Cloning the resulting repository
To clone the resulting repository, simply add a `--recursive` flag to the `git clone` command that you would normally use.  For example, the following command will clone the `dn-solr` repository from the `Datanexus` organization at GitHub.com (which includes this repository as a submodule):

```bash
$ git clone --recursive https://github.com/Datanexus/dn-solr
```

# Pulling changes into the resulting repository
If this repository is modified and the developer of a repository that includes this one as a submodule wants to pull the latest changes into their repository, they can do that quite easily by using a sequence of commands that look something like this:

```bash
$ cd common-flows
$ git checkout master
    ...
$ git pull
$ cd ..
$ git add common-flows
    ...
$ git commit -m "updating common-flows submodule version"
    ...
$ git push
    ...
```

This sequence of commands will pull down the latest changes from the `https://github.com/Datanexus/common-flows` repository, update the version of the `common-flows` submodule that is used by the  repository that depends on it, and commit that new version to that repository.
