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

### Step 3a - Create the AAP Admin Credential
A custom credential is currently needed to work around an issue with the ansible.platform collection being run from an Ansible Automation Platform job template.  This step will be removed once the issue has been resolved
1. Navigate to the **Automation Execution -> Infrastructure -> Credential Types** section in the left hand sidebar.
2. Click the **Create credential type** button and use the following values to create the custom credential type.

|      |                       |
|------|-----------------------|
| Name | 'Product Demos Bootstrap Credential' |
| Description | 'Custom credential type for bootstrapping Ansible Product Demos' |
| Input configuration | *See below* |
| Injector configuration | *See below* |

Use the following YAML block for the input configuration:
```yaml
---
fields:
  - id: host
    type: string
    label: Red Hat Ansible Automation Platform URL
  - id: username
    type: string
    label: Username
  - id: password
    type: string
    label: Password
    secret: true
  - id: oauth_token
    type: string
    label: OAuth Token
    secret: true
  - id: verify_ssl
    type: boolean
    label: Verify SSL
required:
  - host
```

Use the following YAML block for the injector configuration:
```yaml
---
env:
  TOWER_HOST: '{{host}}'
  TOWER_USERNAME: '{{username}}'
  TOWER_PASSWORD: '{{password}}'
  TOWER_VERIFY_SSL: '{{verify_ssl}}'
  TOWER_OAUTH_TOKEN: '{{oauth_token}}'
  CONTROLLER_HOST: '{{host}}'
  CONTROLLER_USERNAME: '{{username}}'
  CONTROLLER_PASSWORD: '{{password}}'
  CONTROLLER_VERIFY_SSL: '{{verify_ssl}}'
  CONTROLLER_OAUTH_TOKEN: '{{oauth_token}}'
  GATEWAY_HOSTNAME: '{{host}}'
  GATEWAY_USERNAME: '{{username}}'
  GATEWAY_PASSWORD: '{{password}}'
  GATEWAY_VERIFY_SSL: '{{verify_ssl}}'
  GATEWAY_OAUTH_TOKEN: '{{oauth_token}}'
```

### Step 3b- Create the AAP Admin Credential
1. Navigate to the **Automation Execution -> Infrastructure -> Credentials** section in the left hand sidebar.
2. Click the **Create credential** button at the top of the screen and use the following values to create your credential.

|      |                       |
|------|-----------------------|
| Name | 'Product Demos Bootstrap Credential' |
| Description | 'Credential for Product Demos Bootstrap' |
| Organization | **Leave this field blank** |
| Credential Type | 'Product Demos Bootstrap Credential' |
| Red Hat Ansible Automation Platform | *Ansible Automation Platform URL* |
| Username | 'admin' |
| Password | *AAP admin password* |

### Step 4 - Edit the Demo Job Template
1. Navigate to the **Automation Execution -> Templates** section in the left hand sidebar.
2. Click on the **Demo Job Template** and edit it. Change the **playbook** field to `install_product_demos.yml`. If you do not see this option, go back to step 2 and ensure your project is configured and synced properly.
3. Click the drop-down arrow on the **Credentials** field, and select `Product Demos Bootstrap Credential` from the list.
4. The **Extra variables** field can be used to defined optional variables as needed:
   - To use an alternate branch in the Ansible Product Demos repo, define the extra variable `apd_git_repo_branch: <your-branch>`
   - To use the Ansible Automation Platform 2.5 Execution environment, define the extra variable `apd_ee_image_version: 25`
5. Save the job template.

### Step 5 - Launch the SETUP Job
1. Navigate back to the **Automation Execution -> Templates** section in the left hand sidebar.
2. Locate the **Product Demos | Single demo setup** job template and click the rocket ship icon on the right to launch the job.
3. Select the use case from the dropdown that you are interested in and continue.


#### You have now completed the setup. Refer to [Ansible Product Demos](https://github.com/ansible/product-demos) for further documentation.
