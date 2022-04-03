## INTRODUCING DYNAMIC ASSIGNMENT INTO OUR STRUCTURE
- In your https://github.com/<your-name>/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.

- Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml. We will instruct site.yml to include this playbook later. 
- Paste the instruction below into the env-vars.yml file.
    
                      
                      ---
                      - name: collate variables from env specific file, if it exists
                        hosts: all
                        tasks:
                          - name: looping through list of available files
                            include_vars: "{{ item }}"
                            with_first_found:
                              - files:
                                  - dev.yml
                                  - stage.yml
                                  - prod.yml
                                  - uat.yml
                                paths:
                                  - "{{ playbook_dir }}/../env-vars"
                            tags:
                              - always
  
 #### Update site.yml with dynamic assignments
  site.yml should now look like this.

        ---
        - hosts: all
        - name: Include dynamic variables 
          tasks:
          import_playbook: ../static-assignments/common.yml 
          include: ../dynamic-assignments/env-vars.yml
          tags:
            - always

        -  hosts: webservers
        - name: Webserver assignment
          import_playbook: ../static-assignments/webservers.yml
  
  #### Download Mysql Ansible Role
- You can browse available community roles at ansible galaxy. We will be using a MySQL role developed by geerlingguy.
- On Jenkins-Ansible server make sure that git is installed with git --version, then go to ‘ansible-config-mgt’ directory and run

        git init
        git pull https://github.com/<your-name>/ansible-config-mgt.git
        git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
        git branch roles-feature
        git switch roles-feature
  
- Inside roles directory create your new MySQL role with ansible-galaxy install geerlingguy.mysql and rename the folder to mysql

          mv geerlingguy.mysql/ mysql

 - Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website.

- Now it is time to upload the changes into your GitHub:

      git add .
      git commit -m "Commit new role files into GitHub"
      git push --set-upstream origin roles-feature
  
- You can create a Pull Request and merge it to main branch on GitHub.
  
  
  
