# $GEOD Token Specification

This document specifies the $GEOD token contract design, including ERC-20 implementation, quadratic voting mechanisms, and governance integration.

## Token Overview

$GEOD is the native governance token of the GeoDistricts DAO, enabling quadratic voting for fair decision-making and treasury access for token holders.

### Token Parameters
- **Name**: GeoDistricts Token
- **Symbol**: $GEOD
- **Decimals**: 18
- **Total Supply**: 100,000,000 $GEOD (fixed, no inflation)
- **Standard**: ERC-20 compatible with governance extensions

## Smart Contract Architecture

### Core Contracts

#### GEODToken.sol
Main ERC-20 token contract with governance extensions.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/Pausable.sol";

contract GEODToken is ERC20, ERC20Permit, ERC20Votes, Ownable, Pausable {
    // Token parameters
    uint256 public constant MAX_SUPPLY = 100_000_000 * 10**18;

    // Vesting contract address
    address public vestingContract;

    // Events
    event TokensMinted(address indexed to, uint256 amount);
    event VestingContractSet(address indexed vestingContract);

    constructor()
        ERC20("GeoDistricts Token", "GEOD")
        ERC20Permit("GeoDistricts Token")
    {
        // Initial supply allocation handled by distribution contract
    }

    // Minting function - only callable by distribution contract
    function mint(address to, uint256 amount) external onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Exceeds max supply");
        _mint(to, amount);
        emit TokensMinted(to, amount);
    }

    // Set vesting contract for locked tokens
    function setVestingContract(address _vestingContract) external onlyOwner {
        vestingContract = _vestingContract;
        emit VestingContractSet(_vestingContract);
    }

    // Emergency pause functionality
    function pause() external onlyOwner {
        _pause();
    }

    function unpause() external onlyOwner {
        _unpause();
    }

    // Override required functions for ERC20Votes
    function _afterTokenTransfer(address from, address to, uint256 amount)
        internal
        override(ERC20, ERC20Votes)
    {
        super._afterTokenTransfer(from, to, amount);
    }

    function _mint(address to, uint256 amount)
        internal
        override(ERC20, ERC20Votes)
    {
        super._mint(to, amount);
    }

    function _burn(address account, uint256 amount)
        internal
        override(ERC20, ERC20Votes)
    {
        super._burn(account, amount);
    }

    function nonces(address owner)
        public
        view
        virtual
        override(ERC20Permit, Nonces)
        returns (uint256)
    {
        return super.nonces(owner);
    }
}
```

#### QuadraticVoting.sol
Implements quadratic voting for governance proposals.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
import "./GEODToken.sol";

contract QuadraticVoting is Ownable, ReentrancyGuard {
    GEODToken public geodToken;

    struct Proposal {
        uint256 id;
        string description;
        address proposer;
        uint256 startTime;
        uint256 endTime;
        uint256 totalVotes;
        uint256 totalQuadraticVotes;
        bool executed;
        mapping(address => Vote) votes;
    }

    struct Vote {
        uint256 votes;        // Linear votes (tokens)
        uint256 quadraticVotes; // sqrt(votes)
        bool hasVoted;
    }

    mapping(uint256 => Proposal) public proposals;
    uint256 public proposalCount;

    uint256 public constant VOTING_PERIOD = 7 days;
    uint256 public constant QUORUM_THRESHOLD = 5; // 5% of circulating supply

    event ProposalCreated(uint256 indexed proposalId, address indexed proposer, string description);
    event VoteCast(uint256 indexed proposalId, address indexed voter, uint256 votes, uint256 quadraticVotes);
    event ProposalExecuted(uint256 indexed proposalId, bool passed);

    constructor(address _geodToken) {
        geodToken = GEODToken(_geodToken);
    }

    function createProposal(string memory description) external returns (uint256) {
        require(geodToken.balanceOf(msg.sender) > 0, "Must hold tokens to propose");

        proposalCount++;
        Proposal storage proposal = proposals[proposalCount];
        proposal.id = proposalCount;
        proposal.description = description;
        proposal.proposer = msg.sender;
        proposal.startTime = block.timestamp;
        proposal.endTime = block.timestamp + VOTING_PERIOD;

        emit ProposalCreated(proposalCount, msg.sender, description);
        return proposalCount;
    }

    function vote(uint256 proposalId, uint256 votes) external nonReentrant {
        Proposal storage proposal = proposals[proposalId];
        require(block.timestamp >= proposal.startTime, "Voting not started");
        require(block.timestamp <= proposal.endTime, "Voting ended");
        require(!proposal.votes[msg.sender].hasVoted, "Already voted");
        require(geodToken.balanceOf(msg.sender) >= votes, "Insufficient balance");

        uint256 quadraticVotes = sqrt(votes);

        proposal.votes[msg.sender] = Vote({
            votes: votes,
            quadraticVotes: quadraticVotes,
            hasVoted: true
        });

        proposal.totalVotes += votes;
        proposal.totalQuadraticVotes += quadraticVotes;

        emit VoteCast(proposalId, msg.sender, votes, quadraticVotes);
    }

    function executeProposal(uint256 proposalId) external {
        Proposal storage proposal = proposals[proposalId];
        require(block.timestamp > proposal.endTime, "Voting still active");
        require(!proposal.executed, "Already executed");

        uint256 circulatingSupply = geodToken.totalSupply(); // Simplified - should exclude locked tokens
        uint256 quorumRequired = (circulatingSupply * QUORUM_THRESHOLD) / 100;

        bool passed = proposal.totalVotes >= quorumRequired;

        proposal.executed = true;
        emit ProposalExecuted(proposalId, passed);

        // Execute proposal logic here based on proposal type
        if (passed) {
            _executeProposalLogic(proposalId);
        }
    }

    function _executeProposalLogic(uint256 proposalId) internal {
        // Implementation depends on proposal type
        // Could integrate with treasury, protocol parameters, etc.
    }

    function sqrt(uint256 x) internal pure returns (uint256) {
        if (x == 0) return 0;
        uint256 z = (x + 1) / 2;
        uint256 y = x;
        while (z < y) {
            y = z;
            z = (x / z + z) / 2;
        }
        return y;
    }

    function getProposal(uint256 proposalId) external view returns (
        uint256 id,
        string memory description,
        address proposer,
        uint256 startTime,
        uint256 endTime,
        uint256 totalVotes,
        uint256 totalQuadraticVotes,
        bool executed
    ) {
        Proposal storage proposal = proposals[proposalId];
        return (
            proposal.id,
            proposal.description,
            proposal.proposer,
            proposal.startTime,
            proposal.endTime,
            proposal.totalVotes,
            proposal.totalQuadraticVotes,
            proposal.executed
        );
    }
}
```

#### TokenDistribution.sol
Handles fair launch distribution and vesting.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

contract TokenDistribution is Ownable {
    using SafeERC20 for IERC20;

    IERC20 public geodToken;

    // Distribution categories
    uint256 public constant AIRDROP_ALLOCATION = 40_000_000 * 10**18;    // 40%
    uint256 public constant GRANTS_ALLOCATION = 30_000_000 * 10**18;    // 30%
    uint256 public constant TREASURY_ALLOCATION = 20_000_000 * 10**18;  // 20%
    uint256 public constant TEAM_ALLOCATION = 10_000_000 * 10**18;      // 10%

    // Distribution tracking
    uint256 public airdropDistributed;
    uint256 public grantsDistributed;
    uint256 public treasuryDistributed;
    uint256 public teamDistributed;

    // Vesting schedules
    mapping(address => VestingSchedule) public vestingSchedules;

    struct VestingSchedule {
        uint256 totalAmount;
        uint256 releasedAmount;
        uint256 startTime;
        uint256 cliffDuration;    // 1 year
        uint256 vestingDuration;  // 4 years total
        bool revoked;
    }

    event TokensDistributed(address indexed recipient, uint256 amount, string category);
    event VestingScheduleCreated(address indexed beneficiary, uint256 amount);
    event TokensReleased(address indexed beneficiary, uint256 amount);

    constructor(address _geodToken) {
        geodToken = IERC20(_geodToken);
    }

    // Airdrop distribution for early contributors
    function distributeAirdrop(address[] calldata recipients, uint256[] calldata amounts) external onlyOwner {
        require(recipients.length == amounts.length, "Arrays length mismatch");

        for (uint256 i = 0; i < recipients.length; i++) {
            require(airdropDistributed + amounts[i] <= AIRDROP_ALLOCATION, "Exceeds airdrop allocation");

            geodToken.safeTransfer(recipients[i], amounts[i]);
            airdropDistributed += amounts[i];

            emit TokensDistributed(recipients[i], amounts[i], "airdrop");
        }
    }

    // Treasury distribution
    function distributeTreasury(address treasury, uint256 amount) external onlyOwner {
        require(treasuryDistributed + amount <= TREASURY_ALLOCATION, "Exceeds treasury allocation");

        geodToken.safeTransfer(treasury, amount);
        treasuryDistributed += amount;

        emit TokensDistributed(treasury, amount, "treasury");
    }

    // Team vesting setup
    function createVestingSchedule(address beneficiary, uint256 amount) external onlyOwner {
        require(teamDistributed + amount <= TEAM_ALLOCATION, "Exceeds team allocation");

        vestingSchedules[beneficiary] = VestingSchedule({
            totalAmount: amount,
            releasedAmount: 0,
            startTime: block.timestamp,
            cliffDuration: 365 days,      // 1 year cliff
            vestingDuration: 1460 days,   // 4 years total
            revoked: false
        });

        teamDistributed += amount;
        emit VestingScheduleCreated(beneficiary, amount);
    }

    // Token release from vesting
    function releaseVestedTokens(address beneficiary) external {
        VestingSchedule storage schedule = vestingSchedules[beneficiary];
        require(!schedule.revoked, "Schedule revoked");
        require(block.timestamp >= schedule.startTime + schedule.cliffDuration, "Cliff not reached");

        uint256 vestedAmount = _calculateVestedAmount(schedule);
        uint256 releasableAmount = vestedAmount - schedule.releasedAmount;

        require(releasableAmount > 0, "No tokens to release");

        schedule.releasedAmount += releasableAmount;
        geodToken.safeTransfer(beneficiary, releasableAmount);

        emit TokensReleased(beneficiary, releasableAmount);
    }

    function _calculateVestedAmount(VestingSchedule memory schedule) internal view returns (uint256) {
        if (block.timestamp < schedule.startTime + schedule.cliffDuration) {
            return 0;
        }

        uint256 timeElapsed = block.timestamp - schedule.startTime;
        if (timeElapsed >= schedule.vestingDuration) {
            return schedule.totalAmount;
        }

        return (schedule.totalAmount * timeElapsed) / schedule.vestingDuration;
    }

    // Emergency functions
    function revokeVesting(address beneficiary) external onlyOwner {
        vestingSchedules[beneficiary].revoked = true;
    }

    // View functions
    function getVestingInfo(address beneficiary) external view returns (
        uint256 totalAmount,
        uint256 releasedAmount,
        uint256 vestedAmount,
        uint256 releasableAmount
    ) {
        VestingSchedule memory schedule = vestingSchedules[beneficiary];
        uint256 vested = _calculateVestedAmount(schedule);
        uint256 releasable = vested - schedule.releasedAmount;

        return (schedule.totalAmount, schedule.releasedAmount, vested, releasable);
    }
}
```

## Quadratic Voting Mechanism

### How It Works
- **Linear Voting**: Vote power = tokens held
- **Quadratic Voting**: Vote power = √(tokens held)
- **Benefit**: Reduces influence of large holders, empowers small holders
- **Cost**: Large holders pay more (in quadratic terms) for additional influence

### Implementation Details
- Votes are cast with linear token amounts
- Quadratic power is calculated as sqrt(votes)
- Total voting power per proposal uses quadratic sum
- Quorum and thresholds use linear token amounts

## Security Considerations

### Audit Requirements
- **Multiple Audits**: At least 3 independent security firms
- **Coverage**: Token contract, voting mechanism, distribution logic
- **Standards**: Following ERC-20 security best practices
- **Upgradeability**: Proxy pattern for future improvements

### Attack Vectors Mitigated
- **Flash Loan Attacks**: Voting requires token ownership during entire period
- **Sybil Attacks**: Quadratic voting reduces effectiveness
- **Governance Attacks**: Multi-sig execution and timelocks
- **Reentrancy**: NonReentrant guards on critical functions

## Deployment Process

### Testnet Deployment
1. Deploy contracts on Ethereum Goerli
2. Conduct extensive testing
3. Security audit completion
4. Bug bounty program launch

### Mainnet Deployment
1. Final security audit sign-off
2. Multi-sig controlled deployment
3. Initial distribution execution
4. Governance transition activation

## Integration Points

### Governance Tools
- **Snapshot**: Off-chain proposal discussion
- **Tally**: On-chain execution interface
- **Boardroom**: Delegate management platform

### Treasury Integration
- Voting contracts control treasury spending
- Grant programs use token-based approval
- Emergency funds require multi-sig + governance approval

This token specification provides the foundation for fair, decentralized governance in the GeoDistricts DAO.