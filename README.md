# Crowdfund contract

## Contract features:

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

  * Additional feature would be to return all funds to the funders on failing a project, however this could require a lot of gas. If we want this we should implement this off-chain.

## Getting started on Devnet
TODO