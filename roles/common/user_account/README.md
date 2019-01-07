# user_account role
There where other "user add" roles over the decades - but none of them were able to satisfy me

## Useage
You have to check for the right public SSH keys inside the files folder of this role e.g

    mgilbert.pub
    mmustermann.pub
    vsprenkler.pub


You can call this role from iside your playbook like:

    - hosts: matching_from_inventory_folder
      gather_facts: False
    tasks:
    roles:
    - { role: common/user_account, username: 'mgilbert', usergroups: 'wheel', ssh_public_keyfiles: ['mgilbert.pub'] }

it will generate the user "mgilbert" in the group "wheel" on this system and deploy the users matching key from iside files folder inside this role
