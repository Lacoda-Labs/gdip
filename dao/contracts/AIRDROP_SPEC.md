# $GEOD Airdrop Distribution Specification

This document details the mechanics, eligibility criteria, and implementation of the $GEOD token airdrop distribution.

## Airdrop Overview

### Total Allocation: 40,000,000 $GEOD (40% of total supply)

The airdrop rewards early contributors and community participants who helped build and validate the GeoDistricts protocol and reference implementation.

### Distribution Categories

| Category | Allocation | Amount | Criteria |
|----------|------------|---------|----------|
| **Protocol Contributors** | 37.5% | 15M $GEOD | Core development and specification work |
| **Early Adopters** | 37.5% | 15M $GEOD | Testing, feedback, content creation |
| **Community Participants** | 25% | 10M $GEOD | Forums, social media, organization |

## Eligibility Criteria

### 1. Protocol Contributors (15M $GEOD)

#### GDIP Authors (5M $GEOD)
- **Primary Authors**: Lead writers of GDIP-001 through GDIP-006
- **Reviewers**: Technical reviewers who provided substantive feedback
- **Editors**: Documentation editors and formatters
- **Verification**: GitHub commits, PR reviews, specification discussions

#### Reference Implementation (6M $GEOD)
- **Core Developers**: Backend, frontend, and infrastructure contributors
- **Architecture Designers**: System architects and technical leads
- **Security Researchers**: Audit participants and bug hunters
- **Verification**: GitHub commits, code reviews, issue resolutions

#### Documentation Contributors (2M $GEOD)
- **Technical Writers**: API docs, user guides, tutorials
- **Translators**: Localization and internationalization
- **Diagram Creators**: Architecture and flow diagrams
- **Verification**: Documentation PRs and content quality

#### Testing Contributors (2M $GEOD)
- **QA Engineers**: Test suite development and execution
- **Bug Reporters**: Detailed issue submissions with reproduction steps
- **Performance Testers**: Load testing and optimization
- **Verification**: Test coverage metrics, bug reports, performance data

### 2. Early Adopters (15M $GEOD)

#### Beta Testers (6M $GEOD)
- **Core Testers**: Used geodistricts.org during development phases
- **Feedback Providers**: Submitted detailed bug reports and feature requests
- **Data Validators**: Verified district calculations and boundary accuracy
- **Verification**: User registration, feedback submissions, test data

#### Content Creators (5M $GEOD)
- **Educational Content**: Tutorials, videos, blog posts about objective redistricting
- **Analysis Pieces**: Research articles on gerrymandering and redistricting
- **Tool Builders**: Third-party analysis tools or visualizations
- **Verification**: Content quality, reach metrics, community impact

#### Ambassadors (4M $GEOD)
- **Community Organizers**: Discord/Forum moderators and event organizers
- **Partnership Builders**: Academic, governmental, or NGO connections
- **Media Outreach**: Press releases, interviews, media appearances
- **Verification**: Community growth metrics, partnership outcomes

### 3. Community Participants (10M $GEOD)

#### Forum Contributors (4M $GEOD)
- **Discussion Leaders**: Started valuable technical or governance discussions
- **Help Providers**: Answered questions and mentored new participants
- **Idea Generators**: Proposed protocol improvements or use cases
- **Verification**: Post quality, engagement metrics, community reputation

#### Social Media Contributors (3M $GEOD)
- **Content Sharers**: High-quality posts about GeoDistricts
- **Engagement Builders**: Community management and growth
- **Brand Ambassadors**: Consistent positive representation
- **Verification**: Reach metrics, engagement rates, content quality

#### Event Organizers (2M $GEOD)
- **Meetup Organizers**: Local or virtual community events
- **Workshop Leaders**: Educational sessions and presentations
- **Conference Speakers**: Presentations at relevant conferences
- **Verification**: Attendance numbers, feedback scores, follow-up impact

#### Translators (1M $GEOD)
- **Documentation Translators**: Core docs in multiple languages
- **Interface Localizers**: UI/UX localization efforts
- **Community Translators**: Forum and discussion translations
- **Verification**: Translation quality, usage metrics, community reach

## Distribution Mechanics

### Retroactive Airdrop Approach
- **Evaluation Period**: From project inception to snapshot date
- **Impact Assessment**: Community voting on contribution value
- **Proportional Distribution**: Rewards scaled by verified impact

### Technical Implementation

#### Merkle Tree Distribution
```solidity
contract GEODAirdrop {
    bytes32 public merkleRoot;
    mapping(address => bool) public claimed;

    function claim(bytes32[] calldata proof, uint256 amount) external {
        require(!claimed[msg.sender], "Already claimed");
        require(verifyProof(proof, keccak256(abi.encodePacked(msg.sender, amount))), "Invalid proof");

        claimed[msg.sender] = true;
        geodToken.transfer(msg.sender, amount);

        emit AirdropClaimed(msg.sender, amount);
    }
}
```

#### Eligibility Verification
- **Merkle Proofs**: Gas-efficient verification without storing all data on-chain
- **Off-chain Computation**: Generate proofs from eligibility database
- **Claim Window**: 90 days post-launch with extendable periods

### Claim Process
1. **Eligibility Check**: User verifies inclusion in airdrop
2. **Proof Generation**: Backend provides merkle proof for user's allocation
3. **On-chain Claim**: User submits proof and claims tokens
4. **Verification**: Smart contract validates proof and transfers tokens

## Fair Distribution Principles

### Anti-Sybil Measures
- **Contribution Verification**: All allocations tied to verifiable actions
- **Impact Assessment**: Quality and reach of contributions considered
- **Community Review**: Peer validation of claimed contributions

### Quadratic Considerations
- **Impact Weighting**: Larger contributions don't automatically get disproportionate rewards
- **Community Consensus**: Final allocations subject to community approval
- **Appeal Process**: Transparent dispute resolution for allocation disputes

## Implementation Timeline

### Pre-Snapshot (Weeks 1-8)
- **Data Collection**: Track contributions across all categories
- **Eligibility Review**: Community verification of contributions
- **Allocation Proposals**: Working group proposes reward distributions

### Snapshot & Preparation (Weeks 9-10)
- **Final Snapshot**: Lock eligibility data at specific block height
- **Merkle Tree**: Generate proofs for all eligible addresses
- **Contract Deployment**: Deploy airdrop contract with merkle root

### Launch & Claims (Weeks 11-14)
- **Contract Activation**: Enable claiming functionality
- **Community Announcements**: Public notification of claim process
- **Support Channels**: Help desk for claim issues

### Post-Launch (Weeks 15+)
- **Unclaimed Recovery**: Treasury recovery of unclaimed tokens
- **Appeals Process**: Handle disputed or missed allocations
- **Analysis**: Post-airdrop analysis of distribution effectiveness

## Communication and Education

### Pre-Launch Transparency
- **Criteria Publication**: Clear eligibility guidelines
- **Example Allocations**: Sample reward calculations
- **Appeal Process**: How to dispute allocations

### Claim Support
- **Step-by-step Guides**: Video tutorials and written instructions
- **FAQ Database**: Common questions and troubleshooting
- **Community Support**: Dedicated Discord channels and moderators

### Post-Airdrop Analysis
- **Distribution Reports**: Public breakdown of allocations by category
- **Impact Assessment**: Analysis of airdrop effectiveness
- **Lessons Learned**: Improvements for future distributions

## Success Metrics

### Distribution Effectiveness
- **Claim Rate**: Percentage of allocated tokens claimed
- **Participation Diversity**: Distribution across contributor categories
- **Community Satisfaction**: Surveys on fairness perception

### Long-term Impact
- **Retention Rate**: Continued participation from airdrop recipients
- **Governance Engagement**: Voting participation from airdrop recipients
- **Ecosystem Growth**: Increased contributions post-airdrop

This airdrop specification ensures fair and impactful distribution of $GEOD tokens to the community that built and validated the GeoDistricts protocol.