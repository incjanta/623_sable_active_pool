# Confirmed live finding: USDS approve race allows user balance drain

## Classification

- Scope: user-specific theft
- Status: confirmed live finding
- Asset stolen: `USDS`
- Who loses: one victim per raced allowance update
- Maximum drain per victim transaction sequence: `oldAllowance + newAllowance`

## Why this is a theft path

`USDSToken.approve()` overwrites the allowance directly, and `transferFrom()` spends whatever allowance is present at execution time. There is no requirement that a spender's existing non-zero allowance be zeroed before a new non-zero value is set.

Affected source lines:

- `src/USDSToken.sol:139-148`
- `approve(address spender, uint256 amount)` writes the new allowance directly.
- `transferFrom(address sender, address recipient, uint256 amount)` transfers first, then decreases the allowance that existed for the spender in that call.

## Preconditions

1. The victim already approved the attacker or attacker-controlled spender for a non-zero amount `A`.
2. The victim submits a new transaction `approve(spender, B)` with `B > 0`.
3. The attacker can front-run or sandwich the victim's approval update.

## Exact exploit sequence

1. Victim has an existing allowance `A` for the attacker.
2. Victim broadcasts `approve(attacker, B)`.
3. Attacker front-runs with `transferFrom(victim, attackerRecipient, A)`, consuming the old allowance.
4. Victim's `approve(attacker, B)` lands and resets allowance to `B`.
5. Attacker spends `B` more with a second `transferFrom`.

## Impact

- The attacker drains `A + B` from the victim instead of being limited to either the old or the new allowance.
- This is a user-asset theft path, not a protocol-TVL drain path.

## Reproduction

Foundry proof of concept:

- `test/USDSTokenApproveRaceAudit.t.sol`

The test mints `200e18` USDS to a victim, spends `100e18` under the old approval, lets the victim reset allowance to `50e18`, then spends the additional `50e18`. Final balances confirm the attacker-controlled recipient receives `150e18`.
