# Sable Fund-Extraction Security Scope

## Paid Impact Focus

This program should only generate, validate, and report findings that match one
of these two impact families:

- Critical fund extraction or protocol value drain: an unprivileged user can
  gain, mint, borrow, redeem, withdraw, claim, or extract more BNB, USDS, SABLE,
  SABLE_LP, collateral, debt value, liquidation value, redemption value, or fee
  value than intended from the protocol.
- Critical reward extraction or unfair reward access: an unprivileged user can
  access rewards they should not receive, bypass reward eligibility or timing,
  claim more rewards than their entitlement, repeat/replay reward claims, or
  otherwise extract excess reward value from StabilityPool, CommunityIssuance,
  SableStakingV2, front-end, BNB, USDS, SABLE, or LP reward paths.

Everything else is out of scope unless the same reachable path directly creates
attacker-controlled fund or reward extraction.

## Out of Scope & Rules

These exclusions are interpreted under the paid impact focus above. A report is
valid only if it proves concrete fund extraction, protocol value drain, reward
extraction, or unfair reward access.

### General

- Impacts requiring attacks that the reporter has already exploited themselves, leading to damage.
- Impacts caused by attacks requiring access to leaked keys/credentials.
- Impacts caused by attacks requiring access to privileged addresses (governance, strategist), except in cases where the contracts are intended to have no privileged access to functions that make the attack possible.
- Impacts relying on attacks involving the depegging of an external stablecoin where the attacker does not directly cause the depegging due to a bug in code.
- Mentions of secrets, access tokens, API keys, private keys, etc. in GitHub will be considered out of scope without proof that they are in use in production.
- Best practice recommendations.
- Feature requests.
- Impacts on test files and configuration files, unless stated otherwise in the bug bounty program.
- Denial of service, liveness failures, liquidation blockage, temporary or
  permanent freezes, griefing, gas griefing, or unavailable functions without
  attacker value extraction.
- Generic high, medium, critical, accounting-desync, invariant, or best-practice
  claims without a concrete path to attacker-controlled funds or rewards.
- Pure insolvency, bad debt, or accounting mismatch unless the attacker can
  withdraw, borrow, redeem, liquidate, claim, or otherwise retain excess value.

### Smart Contracts / Blockchain DLT

- Incorrect data supplied by third-party oracles.
- Impacts requiring basic economic and governance attacks (e.g. 51% attack).
- Lack of liquidity impacts.
- Impacts from Sybil attacks.
- Impacts involving centralization risks.
- Oracle, price, rounding, fee, or decimal issues unless they let an
  unprivileged attacker extract funds/rewards or increase attacker-controlled
  value.
- Liquidation or redemption correctness issues unless they let an attacker seize
  too much collateral, redeem too much value, avoid required repayment while
  keeping value, or overclaim liquidation/redemption/gas-compensation gains.
- Reward accounting issues unless they let an attacker claim more BNB, USDS,
  SABLE, LP-derived value, front-end rewards, staking rewards, or
  StabilityPool rewards than their entitlement.

Note: This does not exclude oracle manipulation/flash-loan attacks when they
directly create fund extraction or reward extraction.

### Websites and Apps

- Theoretical impacts without any proof or demonstration.
- Impacts involving attacks requiring physical access to the victim device.
- Impacts involving attacks requiring access to the local network of the victim.
- Reflected plain text injection (e.g. URL parameters, path, etc.).
- This does not exclude reflected HTML injection with or without JavaScript.
- This does not exclude persistent plain text injection.
- Any impacts involving self-XSS.
- Captcha bypass using OCR without impact demonstration.
- CSRF with no state-modifying security impact (e.g. logout CSRF).
- Impacts related to missing HTTP security headers (such as `X-FRAME-OPTIONS`) or cookie security flags (such as `httponly`) without demonstration of impact.
- Server-side non-confidential information disclosure, such as IPs, server names, and most stack traces.
- Impacts causing only the enumeration or confirmation of the existence of users or tenants.
- Impacts caused by vulnerabilities requiring unprompted, in-app user actions that are not part of the normal app workflows.
- Lack of SSL/TLS best practices.
- Impacts that only require DDoS.
- UX and UI impacts that do not materially disrupt use of the platform.
- Impacts primarily caused by browser/plugin defects.
- Leakage of non-sensitive API keys (e.g. Etherscan, Infura, Alchemy, etc.).
- Any vulnerability exploit requiring browser bugs for exploitation (e.g. CSP bypass).
- SPF/DMARC misconfigured records.
- Missing HTTP headers without demonstrated impact.
- Automated scanner reports without demonstrated impact.
- UI/UX best practice recommendations.
- Non-future-proof NFT rendering.

### Explicitly Out of Scope For This Run

- Any denial-of-service finding, even severe, unless the exploit also transfers
  or unlocks excess value for the attacker.
- Any freeze/liveness/blocked-withdrawal issue unless it is part of a direct
  attacker profit path.
- Any liquidation blockage issue unless the attacker extracts collateral,
  repayment value, gas compensation, or rewards beyond entitlement.
- Any admin-only, timelock-only, governance-only, or owner-only action unless an
  unprivileged user can trigger a later extraction path from it.
- Any report whose impact is only "the protocol is unhealthy", "accounting is
  wrong", or "state is inconsistent" without proving excess attacker-controlled
  value.

## Prohibited Activities

The following activities are prohibited by default on bug bounty programs on Immunefi. Projects may add further restrictions to their own program.

- Any testing on mainnet or public testnet deployed code; all testing should be done on local forks of either public testnet or mainnet.
- Any testing with pricing oracles or third-party smart contracts.
- Attempting phishing or other social engineering attacks against employees and/or customers.
- Any testing with third-party systems and applications (e.g. browser extensions), as well as websites (e.g. SSO providers, advertising networks).
- Any denial-of-service attacks that are executed against project assets.
- Automated testing of services that generates significant amounts of traffic.
- Public disclosure of an unpatched vulnerability in an embargoed bounty.
