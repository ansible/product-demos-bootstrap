# product-demos-bootstrap
Bootstrap playbook for adding [Ansible Product Demos](https://github.com/ansible/product-demos) to an existing Ansible Automation Platform (AAP) environment.

## Using this Repo

### Step 1 - Install Ansible Automation Platform
Presumably you have already completed this step before arriving here, however you need to install Ansible Automation Platform before you continue.

### Step 2 - Edit the Demo Project
1. Log in to your Automation Automation Platform as the `admin` user
2. Navigate to **Automation Execution -> Projects** in the left hand sidebar.
3. If you have the **Demo Project** created at installation time:
   1. Click on the **Demo Project** and edit it.
   2. Change the **Source Control URL** to `https://github.com/ansible/product-demos-bootstrap` and save the project.
   3. Make sure the project sync completes.
4. If you no longer have the **Demo Project**:
   1. Click the **Add** button at the top of the **Projects** menu.
   2. Name the project **Product Demos Bootstrap**
   3. Choose "git" for the **Source Control Type**
   4. In the **Source Control URL** box, use `https://github.com/ansible/product-demos-bootstrap` as the input and save the project.
   5. Make sure the project sync completes.

### Step 3 - Create the AAP Admin Credential
1. Navigate to the **Automation Execution -> Infrastructure -> Credentials** section in the left hand sidebar.
2. Click the **Add** button at the top of the screen and use the following values to create your credential.

|      |                       |
|------|-----------------------|
| Name | 'AAP Admin Credential' |
| Organization | **Leave this field blank** |
| Credential Type | 'Red Hat Ansible Automation Platform' |
| Red Hat Ansible Automation Platform | *Ansible Automation Platform URL* |
| Username | 'admin' |
| Password | *admin password* |

### Step 4 - Edit the Demo Job Template
1. Navigate to the **Automation Execution -> Templates** section in the left hand sidebar.
2. Click on the **Demo Job Template** and edit it. Change the **playbook** field to `install_product_demos.yml`. If you do not see this option, go back to step 2 and ensure your project is configured and synced properly.
3. Click the drop-down arrow on the **Credentials** field, and select `AAP Admin Credential` from the list.
4. The **Extra variables** field can be used to defined optional variables as needed:
   - To use an alternate branch in the Ansible Product Demos repo, define the extra variable `apd_git_repo_branch: <your-branch>`
   - To use the Ansible Automation Platform 2.5 Execution environment, define the extra variable `apd_ee_image_version: 25`
5. Save the job template and click **Launch**

### Step 5 - Launch the SETUP Job
1. Navigate back to the **Automation Execution -> Templates** section in the left hand sidebar.
2. Locate the **Product Demos | Single demo setup** job template and click the rocket ship icon on the right to launch the job.
3. Select the use case from the dropdown that you are interested in and continue.


#### You have now completed the setup. Refer to [Ansible Product Demos](https://github.com/ansible/product-demos) for further documentation.
