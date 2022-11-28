# How to configure FortiGate VPN to use IBM Security Verify SAML Single sign-on

This how-to guide walks you through how to configure FortiGate VPN to use IBM Security Verify Single sign-on.

## Before you begin

Before you begin, make sure you have the following prerequisites:

- FortiGate with FortiOS 6.4.0 or later
- FortiClient or FortiClient VPN 6.4.0 or later
- Access to the IBM Security Verify Admin Portal
- Access to the FortiGate admin console or CLI

Before you configure the FortiGate SSL VPN web interface for SSO, make sure you have the following:

- FortiGate Fully-qualified domain: https://vpn. [your-domain-name].com
- You have created a FortiGate admin and users for SSO

## Simple example to get you started
Remember, this is meant to be simple authentication/authorisation example just to get you started. We're assuming you have already configured things like your Fortinet SSL certificate and your access-control policies. In fact, we're assuming you already have your VPN configured using either local authentication, LDAP or RADIUS authentication. Our goal is to simply add SAML as an additional option.

## See mistakes/bugs in our examples?
If you notice an issue with our documentation, please let us know. We want to fix everything. Just raise an issue in Github, and we'll address it.