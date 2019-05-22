
[![CircleCI](https://img.shields.io/circleci/project/github/ncar-hackathons/cookiecutter-project-template/master.svg?style=for-the-badge&logo=circleci)](https://circleci.com/gh/ncar-hackathons/cookiecutter-project-template)


# cookiecutter-project-template
Cookiecutter project template for ncar-hackathons based projects

```bash
pip install --user cookiecutter
cookiecutter https://github.com/ncar-hackathons/cookiecutter-project-template.git
```

## Provisioning a Deploy Key

Once a new repo is created, the key fingerprint entry in `.circleci/config.yml` needs to be updated:

https://github.com/ncar-hackathons/cookiecutter-project-template/blob/5fb69528e292150e94e3bb32477d61d3c54ae618/%7B%7Bcookiecutter.project_slug%7D%7D/.circleci/config.yml#L29-L31


```console
 fingerprints: 
   - "1d:76:3a:97:e8:e9:b1:a2:29:ba:46:e3:dd:84:d3:7f" 
```


To get new key fingerprint, we need to provision a read/write deploy key. This is an ssh key pair specific to a single repository rather than a user. This is nice for teams, because it means access doesn’t disappear if the user who provisions the key leaves the organization or deletes their account. It also means that any user who is an administrator on the account can follow the steps below to get the integration set up.

We start by creating an ssh key pair on our local machine:

```bash
ssh-keygen -m PEM -t rsa -C "your_email@example.com"  # force PEM format
# Accept the default of no password for the key (This is a special case!)
# Choose a destination such as ci_deploy_key_rsa'
```

We end up with a private key `ci_deploy_key_rsa` and a public key `ci_deploy_key_rsa.pub`. We hand over the private key to CircleCI by navigating to `https://circleci.com/gh/ncar-hackathons/repo-name/edit#ssh`, hitting `Add SSH Key`, entering `github.com` as the hostname, and pasting in the contents of the private key file. 

At this point, we can go ahead and delete the private key from our system, as only our CircleCI project should have access.

The  `https://circleci.com/gh/ncar-hackathons/repo-name/edit#ssh` page will show us the fingerprint for our key, which is a unique identifier that’s safe to expose publicly.



Now, we need to upload the public key to GitHub so that it knows to trust a connection from CircleCI initiated with our private key. We head to `https://github.com/ncar-hackathons/repo-name/settings/keys` > Add Deploy Key, make the title of it “CircleCI write key” and paste in the contents of `ci_deploy_key_rsa.pub` and allow write access to this key. If you haven’t already deleted the private key, be extra careful you’re not accidentally copying from `ci_deploy_key_rsa`! 
