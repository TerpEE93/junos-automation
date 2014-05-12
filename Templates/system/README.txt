config_system template:
    Automagically generates the system{} stanza of a Junos config.
    Reads in the template from config_system,j2 and the variable
    data from datavars.yml, and dumps the output to the screen.

    This is not a complete solution for every application, but
    the content should be sufficient to serve as an aid for a more
    extensive (and customized) implementation.

    Cool idea: I'd like to find a way to split the datavars.yml
    file in two -- one file for global variables, and one for
    device-specific variables.  However, it doesn't seem that
    there is a YAML library that supports merging of two or
    more documents into a single document.  At least, there's
    nothing obvious to a non-programmer like me.

    Update:  I've been told the idea above is best addressed
    via a tool like Ansible.  I have that loaded now, so I'll
    start playing with it.