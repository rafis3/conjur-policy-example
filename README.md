# Sequence #

- Create root: `conjur policy load root root.yml`

- Create Vault: `conjur policy load root vault.yml`
- Create lob: `conjur policy load vault vault-lob1.yml`
  - Create safes under lob:

  ```bash
  conjur policy load vault/lob1 vault-lob1-safe1.yml
  conjur policy load vault/lob1 vault-lob1-safe2.yml
  conjur policy load vault/lob1 vault-lob1-safe3.yml
  ```

  - Create deledation policies:

  ```bash
  conjur policy load vault/lob1/safe1 vault-lob1-safe1-delegation.yml
  conjur policy load vault/lob1/safe2 vault-lob1-safe2-delegation.yml
  conjur policy load vault/lob1/safe3 vault-lob1-safe3-delegation.yml
  ```

  - Create the consumers group in each delegation policy:

  ```bash
  conjur policy load vault/lob1/safe1/delegation consumers.yml
  conjur policy load vault/lob1/safe2/delegation consumers.yml
  conjur policy load vault/lob1/safe3/delegation consumers.yml
  ```

  - Create the variables in the safe policies:

  ```bash
  conjur policy load vault/lob1/safe1 vault-lob1-safe1-vars.yml
  conjur policy load vault/lob1/safe2 vault-lob1-safe2-vars.yml
  conjur policy load vault/lob1/safe3 vault-lob1-safe3-vars.yml
  ```

  - Add values to variables in each safe:

  ```bash
  conjur variable values add vault/lob1/safe1/account1/username user1
  conjur variable values add vault/lob1/safe1/account1/password password1
  conjur variable values add vault/lob1/safe1/account2/username user2
  conjur variable values add vault/lob1/safe1/account2/password password2
  conjur variable values add vault/lob1/safe1/account3/username user3
  conjur variable values add vault/lob1/safe1/account3/password password3

  conjur variable values add vault/lob1/safe2/account1/username user1
  conjur variable values add vault/lob1/safe2/account1/password password1
  conjur variable values add vault/lob1/safe2/account2/username user2
  conjur variable values add vault/lob1/safe2/account2/password password2
  conjur variable values add vault/lob1/safe2/account3/username user3
  conjur variable values add vault/lob1/safe2/account3/password password3

  conjur variable values add vault/lob1/safe3/account1/username user1
  conjur variable values add vault/lob1/safe3/account1/password password1
  conjur variable values add vault/lob1/safe3/account2/username user2
  conjur variable values add vault/lob1/safe3/account2/password password2
  conjur variable values add vault/lob1/safe3/account3/username user3
  conjur variable values add vault/lob1/safe3/account3/password password3
  ```

- Create cf: `conjur policy load root root-cf.yml`
  - Create cf policies:

  ```bash
  conjur policy load cf root-cf-org1.yml
  conjur policy load cf root-cf-org2.yml
  conjur policy load cf root-cf-org3.yml
  ```

  - Grant one of the CF space hosts permission to the variables in `safe1`: `conjur policy load vault/lob1/safe1/delegation vault-lob1-safe1-delegation-consumers.yml`

- Create the K8s policies:

  - Load the `authn-k8s` policy into the `conjur` policy: `conjur policy load conjur conjur-authn-k8s.yml`

  - Load the authn-k8s authenticator instance policy into the `authn-k8s` policy: `conjur policy load conjur/authn-k8s conjur-authn-k8s-prod.yml`

  - Load the hosts of the apps1 team: `conjur policy load conjur/authn-k8s/prod conjur-authn-k8s-prod-apps1.yml`

  - Grant the hosts of the `apps1` team to authenticate with `authn-k8s/prod`: `conjur policy load conjur/authn-k8s/prod conjur-authn-k8s-prod_allowed_apps.yml`

  - Grant one of the K8s hosts permission to the variables in `safe2`: `conjur policy load vault/lob1/safe2/delegation vault-lob1-safe2-delegation-consumers.yml`