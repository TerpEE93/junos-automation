HOW THE ANSIBLE PLAYBOOK FILE AND FOLDER HIERARCHY WORKS
========================================================
This is my best attempt at explaining it.  Hopefully it makes sense...

The Document Root (That's where you found this file!)
-----------------------------------------------------
Put your playbooks here.  You will also find the file env-setup which you can
source from your shell to set some useful environment variables.


The build/ folder
-----------------
This is where we're going to drop configs as we build them.  Each device gets
its own folder under build, and the playbook build.pb.yml will build all of the
necessary folders for us.

The group_vars/ folder
----------------------
Got a variable where the value stays the same for more than one device?  Great!
Create a file for it in the group_vars folder so that it can be associated with
all the devices that share that value.  Now, if you want a play to pull values
from variables in a file under group_vars, that file needs to share its name
with the hosts entry under the play.  For example, if I have a play that
creates a config file that is common to all Junos platforms, and it looks
something like this:

- name: Build base system for Junos devices
  hosts: junos
  connection: local
  gather_facts: no
  roles:
    - base_system

then I'd better have a file named junos.yml in the group_vars folder.

The host_vars/ folder
---------------------
Got a variable where the value is different for every device?  Great!  That
variable belongs in a file in the host_vars folder.  Just like with the
group_vars folder, names matter in host_vars.  Specifically, the name of the
file here has to match the name of the host in your inventory.  So if you
have host-specific variables to apply to a host that Ansible knows as "coke",
then you need to name that file coke.yml.

The hosts/ folder
-----------------
This is where your host definitions grow.  They can all be listed in a single
file, or each type/group/etc. can have its own file.  You decide what works
best for you.

The packages/ folder
--------------------
Got something like a software package that you want to keep with everything?
Just put it here.  The packages folder is simply a holding pen.

The roles/ folder
-----------------
This is where the heavy lifting happens.  This folder includes a bunch of
sub-folders, each pertaining to a specific role.  What is a role?  Best I can
explain is that it's a series of tasks or configuration that applies to a group
of objects.  So a role could be "router" or "firewall".  But it could also be
broader or more specific.  For example, I have a role called base_system that
builds config common to all Junos platforms.  I could have a role for firewall,
but I could also have more specific roles for data center firewalls, DMZ
services firewalls, etc.  Any object can be assigned to one or more roles.

Under the roles/<role_name> folder, you will find folders named files, tasks,
templates, and vars.  Not all may be present, depending on what you need for
the role to operate.  Every role has at least one task associated with it, so
you should always have a tasks folder with at least one file included there.

Tasks generally work on templates, and that's just what you'll find under the
templates folder.

The files folder is optional.  Think of it as a holding pen for data you don't
want to mix in with other templates or variables.

The vars folder is also optional.  It's another place to put variables that are
specific to the role.