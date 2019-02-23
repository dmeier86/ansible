## sudoers-role
Example:

    - hosts: "tag_environment_Test:&tag_application_foo"
      gather_facts: True 
      become: true
    
      roles:
        - role: "roles/common/sudoers"
          sudoer_aliases:
    
          user:
             - name: "ADMIN"
               comment: "Group of admin users"
               users:
                 - "%adm"
           runas:
             - name: "ROOT"
               comment: "Root stuff"
               users:
                 - "#0"
           host:
             - name: "SERVERS"
               comment: "XYZ servers"
               hosts:
                 - "192.168.0.1"
                 - "192.168.0.2"
           command:
             - name: "ADMIN_CMNDS"
               comment: "Stuff admins need"
               commands:
                 - "/usr/bin/passwd"
                 - "/usr/bin/useradd"
                 - "/usr/bin/userdel"
                 - "/usr/bin/usermod"
                 - "/usr/bin/visudo"
    
        
          sudoer_specs:
            - name: "adminis"
              comment: "Stuff for admins"
              users: "ADMIN"
              hosts: "SERVERS"
              operators: "ROOT"
              tags: "NOPASSWD"
              commands: "ADMIN_CMNDS"
    
      tasks:
      (...)
and you'll get the following **/etc/sudoers**:

    # Ansible managed
    
    Defaults    requiretty
    Defaults    !visiblepw
    Defaults    always_set_home
    Defaults    env_reset
    Defaults    env_keep = "COLORS DISPLAY HOSTNAME HISTSIZE INPUTRC"
    Defaults    env_keep += "KDEDIR LS_COLORS MAIL PS1 PS2"
    Defaults    env_keep += "QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
    Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES LC_MONETARY"
    Defaults    env_keep += "LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE LC_TIME"
    Defaults    env_keep += "LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"
    Defaults    secure_path = "/sbin:/bin:/usr/sbin:/usr/bin"
    
    ## User Aliases
    ## These aren't often necessary, as you can use regular groups
    ## (ie, from files, LDAP, NIS, etc) in this file - just use %groupname
    ## rather than USERALIAS
    # Group of admin users
    User_Alias ADMINS = %adm
    
    ## Runas Aliases
    # Root stuff
    Runas_Alias ROOT = #0
    
    ## Host Aliases
    ## Groups of machines. You may prefer to use hostnames (perhaps using
    ## wildcards for entire domains) or IP addresses instead.
    # XYZ servers
    Host_Alias SERVERS = 192.168.0.1,192.168.0.2
    
    ## Command Aliases
    ## These are groups of related commands...
    # Stuff admins need
    Cmnd_Alias ADMIN_CMNDS = /usr/bin/passwd,/usr/bin/useradd,/usr/bin/userdel,/usr/bin/usermod,/usr/bin/visudo
    
    
    ## Allow root to run any commands anywhere
    root	ALL=(ALL) 	ALL
    
    ## Read drop-in files from /etc/sudoers.d (the # here does not mean a comment)
    #includedir /etc/sudoers.d
and this **/etc/sudoers.d/adminis** file:

    # Ansible managed
    
    # Stuff for admins
    Defaults:ADMIN !requiretty
    ADMIN SERVERS=(ROOT) NOPASSWD: ADMIN_CMNDS

There's more you can learn 'bout sudoers file:
 - https://help.ubuntu.com/community/Sudoers
