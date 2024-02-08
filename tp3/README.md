# TP 3

## Question 1 : Document your inventory and base commands
### 1
```bash
ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"
```
This will display the distribution of all the machines in the inventory.
- `all` : all the machines in the inventory
- `-i inventories/setup.yml` : the inventory file
- `-m setup` : the module to use
- `-a "filter=ansible_distribution*"` : the argument to pass to the module, in this case, the filter to apply. The filter is used to display only the distribution of the machines. (ansible_distribution* is a regex that matches all the keys that start with ansible_distribution)

### 2
```bash
ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become
```
- `all` : all the machines in the inventory
- `-i inventories/setup.yml` : the inventory file
- `-m yum` : the module to use, yum is a package manager for RedHat based distributions (equivalent to `apt` or `brew`)
- `-a "name=httpd state=absent"` : the argument to pass to the module, in this case, the package is `httpd` and the state is `absent` (absent means that the package will be removed if it exists)

## Question 2 : Document the playbook
The playhook is [here](playbook.yml)

## Question 3 : Document container tasks
Every docker container is defined almost the same way. The only difference is the name of the container and the port to expose.
There is also some that need to access to environment variables.

## Ansible-vault
- Create a vault: `ansible-vault create path/to/vault.yml`
- Edit a vault: `ansible-vault edit path/to/vault.yml`
- View a vault: `ansible-vault view path/to/vault.yml`

You create a vault file and put in it the variables you want to encrypt. Then you can use these variables in your playbook with the following syntax:
```yaml
- name: "Create a container"
  docker_container:
    name: "{{ container_name }}"
    image: "{{ container_image }}"
    state: started
    ports:
      - "{{ container_port }}:{{ container_port }}"
    env:
      - "{{ container_env_var }}"
```
When you deploy something, you will have to use the `--ask-vault-pass` option to provide the password to decrypt the vault.

## The Github Actions
The github actions that use our ansible playbook is [here](.github/workflows/ansible.yml), it's documented in the file.

## The HTTP Server
Because the HTTP server should allow you to access the content of the front and the API, I decided to root everything that starts with `/api` to the API and everything else to the front. I'm not really using the port like I should, but I think it's a good way to do it.

## Proxy Authentication
I set up a proxy authentication because everyone could access it and insert data. So I didn't want this to append. I used the `auth_basic` and `auth_basic_user_file` to set up the authentication. I also used the `htpasswd` command to create the file that contains the users and their passwords.
That wasn't asked but I was too curious to not do it.


## Flaws
- The Front is not quite variablized.
- I should have more networks:
    - One for between the front and the API
    - One for the API and the database
    - One for the front and the http server
    - Maybe, if asked, one for the http server and the API
- I didn't use a volume for the database, so if the container is removed, the data is lost.
    - I did this on purpose, because i know that the database is not important for this project, but in a real project, I would have used a volume. In addition, it was useful to have a fresh database until the proxy authentication was set up.


## Environment
- Setup python env: `python3 -m venv ansible`
- Activate python env: `source ansible/bin/activate`
- Install ansible: `pipx install --include-deps ansible`
- Check for updates: `pipx upgrade --include-injected ansible`
- Bonus : `pipx inject ansible argcomplete`
- Leave python env: `deactivate`

## Tips
- Become super user: `become: yes`
- Run playhook: `ansible-playbook -i inventories/setup.yml playbook.yml`
