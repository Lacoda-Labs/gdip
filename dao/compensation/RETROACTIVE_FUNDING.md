# GeoDistricts DAO Retroactive Funding Program

This document details the retroactive funding mechanism for rewarding past contributions to the GeoDistricts ecosystem.

## Program Overview

### Core Concept
Retroactive funding rewards contributors for work that has already benefited the ecosystem, rather than requiring up-front proposals. This approach incentivizes high-impact work without the risk and friction of traditional grant applications.

### Funding Structure
- **Monthly Rounds**: Regular funding cycles every 30 days
- **Budget Allocation**: $50,000 per round (scalable based on treasury growth)
- **Distribution Method**: Proportional allocation based on community-assessed impact
- **Inspiration**: Optimism's retroactive funding model with quadratic elements

## Round Structure

### Phase 1: Contribution Documentation (Days 1-14)
Contributors document their past work that benefited GeoDistricts:
- **Protocol Development**: GDIP creation, specification work
- **Implementation**: Code contributions, bug fixes, features
- **Documentation**: Guides, tutorials, translations
- **Community Building**: Events, education, outreach
- **Research**: Analysis, partnerships, legal work

### Phase 2: Community Review (Days 15-21)
- **Public Voting**: Token holders assess contribution impact
- **Quadratic Scoring**: Voting power = √(tokens held)
- **Review Criteria**: Quality, reach, sustainability, alignment
- **Appeal Period**: 48 hours for disputes or corrections

### Phase 3: Funding Distribution (Days 22-30)
- **Proportional Allocation**: Rewards scaled by impact scores
- **Minimum Threshold**: $100 minimum payout per approved contribution
- **Maximum Cap**: No upper limit (based on demonstrated impact)
- **Payment Method**: Direct $GEOD or stablecoin transfer

## Impact Assessment Framework

### Scoring Dimensions

#### 1. Quality (40% weight)
- **Technical Excellence**: Code quality, documentation clarity, research rigor
- **Innovation**: Novel approaches, creative solutions, paradigm shifts
- **Completeness**: Comprehensive coverage, edge case handling, testing

#### 2. Reach & Impact (35% weight)
- **User Benefit**: How many users benefited from the work
- **Ecosystem Growth**: Broader impact on adoption and development
- **Sustainability**: Long-term value vs. short-term gains

#### 3. Alignment (15% weight)
- **Protocol Goals**: Advancement of objective redistricting mission
- **Community Values**: Fosters decentralization, transparency, fairness
- **Strategic Fit**: Supports DAO's long-term vision

#### 4. Verification (10% weight)
- **Documentation**: Clear evidence and deliverables
- **Community Validation**: Testimonials, usage metrics, citations
- **Reproducibility**: Work can be verified and built upon

### Scoring Algorithm
```javascript
function calculateReward(contribution) {
  const qualityScore = assessQuality(contribution);
  const impactScore = assessImpact(contribution);
  const alignmentScore = assessAlignment(contribution);
  const verificationScore = assessVerification(contribution);

  const weightedScore = (
    qualityScore * 0.4 +
    impactScore * 0.35 +
    alignmentScore * 0.15 +
    verificationScore * 0.1
  );

  // Quadratic adjustment to prevent large holder dominance
  const quadraticAdjustment = Math.sqrt(weightedScore);

  return quadraticAdjustment;
}
```

## Application Process

### Submission Template
```markdown
# Retroactive Funding Application

## Contributor Information
- **Name/Handle**: 
- **Contact**: 
- **Category**: (Protocol/Implementation/Documentation/Community/Research)

## Contribution Summary
- **Title**: Brief description of the work
- **Timeframe**: When the work was performed
- **Deliverables**: Links to code, docs, or other outputs

## Impact Assessment
- **Problem Solved**: What issue was addressed
- **Solution Approach**: How the problem was tackled
- **Results**: Measurable outcomes or benefits

## Supporting Evidence
- **Links**: GitHub PRs, forum posts, publications
- **Testimonials**: Feedback from community members
- **Metrics**: Usage stats, adoption numbers, citations

## Requested Funding
- **Amount**: $X (justification based on effort and impact)
- **Preferred Asset**: $GEOD or stablecoin
- **Distribution**: Lump sum or vesting schedule
```

### Submission Platform
- **Forum Category**: Dedicated retroactive funding section
- **Template Enforcement**: Required fields with validation
- **Edit Period**: 24 hours to modify submissions post-creation
- **Withdrawal Option**: Contributors can withdraw applications if needed

## Review and Voting Process

### Community Review Phase
- **Discussion Period**: 7 days for questions and feedback
- **Expert Review**: Domain specialists provide detailed assessments
- **Community Voting**: Quadratic voting on impact scores
- **Score Aggregation**: Weighted average of expert and community scores

### Voting Mechanics
- **Quadratic Voting**: Vote power = √(tokens held)
- **Scoring Scale**: 1-10 for each dimension
- **Delegation**: Token holders can delegate voting power
- **Minimum Participation**: 3% quorum for round validity

### Appeal Process
- **Initial Appeal**: 48 hours post-voting for scoring disputes
- **Appeal Committee**: 3-member panel reviews contested applications
- **Final Decision**: Binding resolution with explanation
- **Appeal Funding**: 5% reserve for justified score adjustments

## Funding Distribution

### Allocation Algorithm
```javascript
function distributeFunds(applications, totalBudget) {
  const totalWeightedScore = applications.reduce((sum, app) =>
    sum + app.weightedScore, 0
  );

  applications.forEach(app => {
    const proportion = app.weightedScore / totalWeightedScore;
    app.allocation = Math.max(proportion * totalBudget, minimumPayout);
  });

  return applications;
}
```

### Payment Execution
- **Smart Contract**: Automated distribution via merkle tree claims
- **Vesting Options**: Contributors can choose vesting schedules
- **Tax Optimization**: Structured payments to minimize tax burden
- **Multi-chain**: Support for Ethereum and Layer 2 networks

### Minimum and Maximum Constraints
- **Minimum Payout**: $100 to ensure meaningful rewards
- **Maximum Payout**: No hard cap (impact-driven allocation)
- **Round Reserve**: 10% held for appeals and adjustments
- **Carryover**: Unused funds roll to next round

## Program Administration

### Round Timeline
- **Day 1**: Round announcement and submission opening
- **Day 14**: Submission deadline
- **Day 15-21**: Review and voting period
- **Day 22**: Results announcement
- **Day 23-30**: Payment distribution and appeals

### Administration Team
- **Program Manager**: Oversees round execution and quality
- **Review Coordinators**: Manage expert reviewer assignments
- **Community Moderators**: Ensure fair discussion and voting
- **Technical Team**: Maintain submission platform and voting system

### Quality Assurance
- **Duplicate Detection**: Automated checking for overlapping applications
- **Sybil Resistance**: Quadratic voting and contribution verification
- **Fraud Prevention**: Manual review of high-value applications
- **Continuous Improvement**: Post-round analysis and process refinement

## Success Metrics

### Program Health
- **Application Volume**: Number of quality submissions per round
- **Participation Rate**: Percentage of token holders voting
- **Appeal Rate**: Percentage of applications contested
- **Satisfaction Score**: Contributor feedback surveys

### Ecosystem Impact
- **Contribution Diversity**: Range of contribution types and categories
- **Protocol Advancement**: Measurable improvements from funded work
- **Community Growth**: Increase in active contributors and participants
- **Sustainability**: Long-term engagement and retention rates

## Future Enhancements

### Advanced Features
- **Automated Scoring**: AI-assisted impact assessment
- **Real-time Feedback**: Continuous contribution recognition
- **Cross-round Learning**: Improved scoring based on historical data
- **Integration Incentives**: Bonus rewards for ecosystem integrations

### Scaling Mechanisms
- **Sub-category Rounds**: Specialized rounds for technical, community, research
- **Larger Budgets**: Increased funding as treasury grows
- **Multi-chain Support**: Retroactive funding on Layer 2 networks
- **Global Expansion**: Localization and international contributor support

This retroactive funding program ensures contributors are fairly compensated for impactful work while driving continuous ecosystem development.