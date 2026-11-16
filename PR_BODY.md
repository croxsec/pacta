<!--
Pacta Chain-Feed Mismatch Challenge: 4 x $25 USDC = $100 USDC hard cap, 7-day sprint Nov 15 - Nov 22, 2026.
Rules and the four slot issues: https://pacta.wtf/bounties/
Fill in EVERY field below (replace each <...>), then check it offline before you open the PR:
  python3 tools/verify_bounty_submission.py --body PR_BODY.md
Governance / documentation parameter mismatches only. Never post an exploit PoC here: security vulnerabilities go to
Immunefi / Cantina per https://pacta.wtf/.well-known/security.txt.
-->

## Fixture file

- Fixture: bounties/submissions/slot-4-gmx.json
  Check it with: python3 tools/verify_bounty_submission.py --json bounties/submissions/slot-4-gmx.json

## Bounty slot

- Slot: 4
- Issue: https://github.com/Fractal-Neurons/pacta/issues/24

## 1. chainId & contract address

- chainId: 42161
- Contract address: 0x03e8f708e9C85EDCEaa6AD7Cd06824CeB82A7E68
- Protocol: GMX (GMX Governor, the DAO on-chain governor used in the documented Phase 4 "Tally" execution stage)
- TVL reference: https://api.llama.fi/protocol/gmx
- Pacta slug: n/a

## 2. Pinned finalized block

- blockNumber: 524807346
- blockHash: 0x6c1ab6516c26aa5acc4e6d7c4f85f332bae7db885f982a626f33497aec0296ce
- Prior Pacta pin: n/a

## 3. Deterministic dual-RPC cast proof

```bash
cast call 0x03e8f708e9C85EDCEaa6AD7Cd06824CeB82A7E68 "COUNTING_MODE()(string)" --block 524807346 --rpc-url https://gateway.tenderly.co/public/arbitrum
cast call 0x03e8f708e9C85EDCEaa6AD7Cd06824CeB82A7E68 "COUNTING_MODE()(string)" --block 524807346 --rpc-url https://arb1.arbitrum.io/rpc
```

- RPC A output: "support=bravo&quorum=bravo"
- RPC B output: "support=bravo&quorum=bravo"

Corroborating parameter reads at the same pin (identical on both endpoints):

```bash
cast call 0x03e8f708e9C85EDCEaa6AD7Cd06824CeB82A7E68 "votingDelay()(uint256)" --block 524807346 --rpc-url <rpc>     # 86400
cast call 0x03e8f708e9C85EDCEaa6AD7Cd06824CeB82A7E68 "votingPeriod()(uint256)" --block 524807346 --rpc-url <rpc>   # 432000
cast call 0x03e8f708e9C85EDCEaa6AD7Cd06824CeB82A7E68 "proposalThreshold()(uint256)" --block 524807346 --rpc-url <rpc> # 30000000000000000000000
cast call 0x4bd1cdAab4254fC43ef6424653cA2375b4C94C0E "getMinDelay()(uint256)" --block 524807346 --rpc-url <rpc>    # 86400
```

## 4. Live official documentation claim (HTTP 200)

- Docs URL: https://docs.gmx.io/docs/governance/proposal-process/
- Verbatim quote: "Voting mechanism: Qualified majority (65% + 1 of the smallest token unit)" — listed under "Phase 4: Tally (Optional)", alongside "Voting delay: 24 hours", "Voting window: 5 days", "Execution delay: 24 hours" and "Minimum tokens to submit a proposal: 30,000".

Doc vs chain: the published Phase-4 decision rule is a 65% qualified majority. The on-chain governor is OpenZeppelin GovernorCountingSimple (COUNTING_MODE "support=bravo&quorum=bravo", source-verified Exact Match on Arbiscan): a proposal succeeds when forVotes > againstVotes — a simple 50%+1 majority. No 65% parameter exists in the contract. The other five published Phase-4 parameters match the deployment exactly (86400 / 432000 / 30000e18 / timelock 86400), confirming the docs describe this exact on-chain governor and the voting-mechanism line is the divergence.

## 5. Pacta MCP output

```json
{
  "schema": "pacta.protocol_lookup.v1",
  "query": "gmx",
  "totalCatalog": { "protocols": 74, "briefs": 8, "easAttestedProtocols": 62, "rpcObservedProtocols": 12 },
  "totalMatches": 0,
  "count": 0,
  "results": [],
  "catalog": "https://pacta.wtf/api/v1/lookup.json",
  "note": "GMX is not present anywhere in the live catalog (verified 2026-11-17 against lookup.json: 0 matches for 'gmx', and absent from protocols.json which lists 74 protocols); no easAttested, rpc_observed, or brief record exists. Target is unindexed.",
  "coverage_gap_hint": "Target is not yet indexed or fully classified."
}
```

## 6. Apache-2.0 contribution & commercialization grant

- [x] I grant Pacta by Fractal Neurons perpetual, irrevocable rights under Apache-2.0 to ingest, Base EAS-attest, publish on pacta.wtf, and commercialize this merged mismatch record.

## Rules

- [x] This is my only open bounty PR (max 1 open PR per contributor across all slots).
- [x] Every RPC output above is real, pinned to the block above, and reproducible on 2 independent endpoints (no AI-generated or fabricated values).
- [x] I have not opened PRs, issues or comments in any third-party repository on Pacta's behalf.
- [x] This PR contains no exploit PoC: it reports a governance / documentation parameter mismatch only.
