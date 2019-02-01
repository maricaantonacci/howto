.. highlight:: console

********************************
Install and configure orchent and oidc-agent
********************************

**orchent** is a command-line tool that facilitates the interaction with the PaaS Orchestrator Service.
In order to submit requests to the Orchestrator you need to provide a token issued by INDIGO IAM for your user; for this purpose, it is suggested the usage of the **oidc-agent** tool that simplifies the management of the OIDC tokens.

In the present guide, you will learn how to configure oidc-agent and then orchent.


1. Installing oidc-agent
------------------------
**oidc-agent** is a tool to manage OpenID Connect tokens and make them easily usable from the command line. Installation instructions and full documentation can be found `here <https://indigo-dc.gitbooks.io/oidc-agent/>`_.

2. Configuring oidc-agent with INDIGO IAM
---------------------------------------------------

.. admonition:: Prerequisites

    * `INDIGO IAM <https://iam-test.indigo-datacloud.eu/>`_ registration


* Start oidc-agent::

	$ eval $(oidc-agent)

* Run::

	$ oidc-gen

You will be asked for the name of the account to configure. Let's call it **eosc-hub**. 
After that you will be asked for the additional client-name-identifier (optional), you should choose the following option for the issuer::

		[1] https://iam-test.indigo-datacloud.eu/

Then just click Enter to accept the default values for Space delimited list of scopes [openid profile offline_access].

* After that, if everything has worked properly, you should see the following messages::

	Registering Client ...
	Generating account configuration ...
	accepted
	
* At this point you will be given a URL. You should visit it in the browser of your choice  in order to continue and approve the registered client. 
* For this you will have to login into your INDIGO IAM account and accept the permissions you are asked for.

* Once you have done this you will see the following message::

	The generated account config was successfully added to oidc-agent. You don't have to run oidc-add

Next time you want to start oidc-agent from scratch, you will only have to do::

	$ eval $(oidc-agent)
	oidc-add eosc-hub
	Enter encryption password for account config eosc-hub: ********
	success

In alternative you can make oidc-agent persistent including this line in your .bashrc:

	test -e ~/tmp/oidc-agent.env && . ~/tmp/oidc-agent.env
	
Then run the agent as follows::

	mkdir ~/tmp/
	oidc-agent > ~/tmp/oidc-agent.env
	source ~/.bashrc

From now on every new shell should have access to the agent.

* Test it - You can print the token::

	$ oidc-token eosc-hub

3. Installing orchent
------------------------
Depending on your linux distribution, install the package from https://github.com/indigo-dc/orchent/releases

4. Configuring orchent
------------------------

*4.1 Using oidc-agent*

* You should set OIDC_SOCK (this is not needed, if you did it before or if you have modified the .bashrc)::

	$ eval $(oidc-agent)
        oidc-add eosc-hub

* Set the agent account to be used with orchent::

	$ export ORCHENT_AGENT_ACCOUNT=eosc-hub

* You also need to set ORCHENT_URL::

	$ export ORCHENT_URL="https://indigo-paas.cloud.ba.infn.it/orchestrator"



