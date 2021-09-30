# CircleCi IKEV2 VPN Orb

[![CircleCI Build Status](https://circleci.com/gh/VerdigrisTech/circleci-orb-vpn.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/VerdigrisTech/circleci-orb-vpn) [![CircleCI Orb Version](https://badges.circleci.com/orbs/verdigris/vpn.svg)](https://circleci.com/orbs/registry/orb/verdigris/vpn) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/VerdigrisTech/circleci-orb-vpn/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)
[Orb-Tools](https://circleci.com/orbs/registry/orb/circleci/orb-tools).


## What is this?

This is a CircleCi '[orb](https://circleci.com/docs/2.0/orb-intro/#quick-start)' (think of it like a package) to use in your CircleCi config.yml in situations where you need to connect to your VPN using the IKEV2 protocol before executing something. 

## Who is this for?

Use this if you already have a VPN that you can access using IKEV2 and most likely are using CircleCI cloud version. 

## How do I use it?
### Prerequisites
- A VPN 
- Access to the VPN using IKEV2 
- A CircleCI context with the access credentials (see example below)

### Example Use

```yaml
version: 2.1

orbs:
  ikev2-vpn: verdigris/ikev2-vpn@<VERSION> #check the latest version
jobs:
  test-connection:
    machine: #IMPORTANT! Must use a machine executor
      image: ubuntu-2004:202101-01
    steps:
      - ikev2-vpn/connect: # context fields
          password: $VPN_PKCS12_PASSWORD  
          hostname: $VPN_HOSTNAME
          p12-b64: $VPN_P12_B64 #Base64 encoded in context
          ca-pem-b64: $VPN_CA_PEM_B64 #Base64 encoded in context
      - run: 
          name: hit a private server
          command: nslookup -query=soa <PRIVATE_DNS> <PRIVATE_IP>

workflows:
  version: 2
  test:
    jobs:
      - test-connection:
          context: vpn
```
Fill in any fields with `<VALUE>` before using.

## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/verdigris/circleci-orb-vpn) - The official registry page of this orb for all versions, executors, commands, and jobs described.
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/VerdigrisTech/circleci-orb-vpn/issues) to and [pull requests](https://github.com/VerdigrisTech/circleci-orb-vpn/pulls) against this repository!

### How to Publish

* Create and push a branch with your new features.
* When ready to publish a new production version, create a Pull Request from _feature branch_ to `master`.
* The title of the pull request must contain a special semver tag: `[semver:<segment>]` where `<segment>` is replaced by one of the following values.

| Increment | Description|
| ----------| -----------|
| major     | Issue a 1.0.0 incremented release|
| minor     | Issue a x.1.0 incremented release|
| patch     | Issue a x.x.1 incremented release|
| skip      | Do not issue a release|

Example: `[semver:major]`

* Squash and merge. Ensure the semver tag is preserved and entered as a part of the commit message.
* On merge, after manual approval, the orb will automatically be published to the Orb Registry.

For further questions/comments about this or other orbs, visit the Orb Category of [CircleCI Discuss](https://discuss.circleci.com/c/orbs).
