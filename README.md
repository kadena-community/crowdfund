# Crowdfund contract

## Contract features

The contract has the following statusses:

- `(defconst CREATED 0)`
- `(defconst CANCELLED 1)`
- `(defconst SUCCEEDED 2)`
- `(defconst FAILED 3)`

### Creating a project

The contract allows anyone to create a project. A project is created with amongst others a title, a token to raise funds in, a hard cap, a soft cap, a start date, and an end date.

### Cancelling a project

Only the creator of a project can cancel a project. Cancelling is only possible before the project has started.

### Funding a project

Anyone can fund a project. Funds are stored in a funds table. Funds are stored with a project id, a fund owner, a status, an amount, and a timestamp.

### Refunding a project

Anyone can ask for a refund as long as the projects status isn't SUCCEEDED yet. When a project is SUCCEEDED funds are distributed and refunds aren't possible anymore.

### Succeeding a project

The project owner can initiate succeeding a project. A project can only be marked as succeeded when either of the two conditions are met:

- the end date has passed and the project has raised more than the soft cap.
- the end date has not passed but the raised amount has reached the hard cap.
  When a project is succeeded the funds are distributed to the beneficiary and the project status is set to SUCCEEDED.

### Failing a project

THe project owner can initiate failing a project. A project can only be marked as failure when the end date has passed and the project has not reached the soft cap. On failing a project the project status is set to FAILED.

- Additional feature would be to return all funds to the funders on failing a project, however this could require a lot of gas. If we want this we should implement this off-chain.

## Getting started on Devnet

To setup devnet we require docker to be installed. After docker is setup we can start devnet with:

```
docker run -it -p 8080:1337 -v $HOME/.devnet/l1:/root/.devenv enof/devnet
```

## Installing kda-cli

To setup kda-cli add the following alias to your `.bashrc` or `.zshrc`:

```
alias kda='docker run --rm -it -v $(pwd):/app/ --add-host=devnet:host-gateway enof/kda-cli'
```

_note: you can change the alias name to whatever suits your needs_

### Funding your account

Before being able to deploy the contract we need to create an account with some KDA on it. The first step is to open Chainweaver and generate a public key. Go to the `Keys` tab and click `Generate Key`. The key will be generated and displayed in the `Keys` tab. Press the `Add k: Account` button to create the account, it will now be visible in the 'Accounts' tab. Copy the account name of the account you just created.

Next, we need to fund the account. To do this we need to invoke the kda-cli with the command: `kda --task=fund`. This will present us with a list of questions which we'll go through below:

#### `What account would you like to fund?`

Paste the account name of the account you just created.

#### `On wich network would you like to fund the account?`

By default the devnet starts with a network called `fast-development`. This is the network we'll use.

#### `On what chain would you like to fund the account?`

Enter a chain number, Kadena contains 20 chains, so use a number between 0 and 19. This chain will also be used to deploy the contract to.

#### `What endpoint would you like to use?`

The devnet is available via docker on `http://devnet:8080`. Enter this endpoint, including the `http` protocol.

#### `What public key would you like to use?`

Enter you public key

You should now have an account with some KDA on it.

### Deploying the contract

To deploy the contract we need to invoke the kda-cli with the command: `kda --task=deploy`

Now we're presented with a list of questions which we'll go through below:

#### `What would you like to deploy?`

Enter the path to a contract file. In this case we'll enter `crowdfund/crowdfund.pact`, taking into consideration the path is relative to the directory you're executing the command from.

#### `What data goes with it?`

Enter the path to a data file in JSON format. This repository contains a default data file `deploy-data-default.json` that can be used.

\*\* Namespaces and keyset are concepts that are explained in the [Kadena Pact documentation](https://pact-language.readthedocs.io/en/latest/pact-reference.html#namespaces).

#### `Where would you like to deploy?`

Select 'L1' from the list of options.

#### `What is the endpoint of L1?`

Enter `http://devnet:8080`.

#### `On which chains on L1 would you like to deploy?`

Select a chain on which to deploy the contract, this should be the chain that you have used previously to fund your account.

#### `What is the account of the signer?`

Enter `sender00` for the signer. This is an account that is pre-installed on every devnet.
