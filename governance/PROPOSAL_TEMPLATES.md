# GeoDistricts DAO Proposal Templates

This document provides standardized templates for different types of proposals in the GeoDistricts DAO governance system.

## Proposal Submission Process

### Pre-Proposal Requirements
1. **Forum Discussion**: Post initial idea in governance forum for feedback
2. **Working Group Review**: For technical proposals, consult relevant working groups
3. **Draft Preparation**: Use appropriate template below
4. **Community Review**: Share draft for final feedback before Snapshot submission

### Snapshot Submission
- **Title Format**: `[Category] Brief Description`
- **Body**: Use template with all required sections
- **Voting Parameters**: 7-day voting period, simple majority (unless constitutional)
- **Execution**: Successful proposals executed via Timelock contract

## Proposal Categories

### 1. Budget Proposals

#### Template Structure
```markdown
# Budget Proposal: [Quarter/Year] Operating Budget

## Summary
Brief description of the proposed budget allocation and priorities.

## Background
Context for why this budget is needed and how it aligns with DAO goals.

## Budget Breakdown

### Development (40% of budget)
- **Protocol Improvements**: $X for GDIP implementation and protocol upgrades
- **Reference Implementation**: $Y for geodistricts.org maintenance and features
- **Tooling**: $Z for developer tools and infrastructure

### Community (25% of budget)
- **Events**: $X for conferences, workshops, and meetups
- **Education**: $Y for content creation and outreach
- **Operations**: $Z for community management and moderation

### Operations (20% of budget)
- **Legal**: $X for legal counsel and compliance
- **Audits**: $Y for smart contract and security audits
- **Insurance**: $Z for DAO insurance coverage

### Ecosystem (10% of budget)
- **Grants**: $X for retroactive funding rounds
- **Partnerships**: $Y for strategic collaborations
- **Marketing**: $Z for ecosystem growth initiatives

### Reserve (5% of budget)
- **Emergency Fund**: $X for unforeseen circumstances
- **Contingency**: $Y for budget adjustments

## Total Budget: $XXX,XXX

## Funding Source
- Treasury Reserve: Primary funding from yield-generating assets
- Grant Programs: Community-directed funding allocation
- Emergency Reserves: Only if absolutely necessary

## Implementation Timeline
- **Month 1**: Initial allocations and program launches
- **Month 2-3**: Program execution and monitoring
- **Quarter End**: Final reporting and impact assessment

## Success Metrics
- **Budget Utilization**: Percentage of allocated funds deployed
- **Program Impact**: Measurable outcomes from funded initiatives
- **Community Feedback**: Stakeholder satisfaction surveys

## Risk Mitigation
- **Spending Limits**: Maximum allocation per category
- **Audit Requirements**: Independent review for large expenditures
- **Contingency Planning**: Backup plans for budget shortfalls

## Voting
- **Quorum**: 5% of circulating supply
- **Threshold**: Simple majority
- **Execution**: Treasury multi-sig with 7-day Timelock
```

### 2. Grant Proposals

#### Retroactive Funding Template
```markdown
# Retroactive Grant: [Project/Initiative Name]

## Summary
Request for retroactive funding for completed work that benefited the GeoDistricts ecosystem.

## Work Completed
Detailed description of work performed and deliverables.

### Timeline
- **Start Date**: When work began
- **Completion Date**: When work was finished
- **Key Milestones**: Major checkpoints achieved

### Impact Assessment
- **Protocol Advancement**: How work improved the protocol
- **Community Benefit**: Value provided to token holders and users
- **Ecosystem Growth**: Broader impact on adoption and awareness

## Funding Request
- **Amount Requested**: $X in $GEOD or stablecoins
- **Payment Schedule**: One-time payment or milestone-based
- **Funding Source**: Retroactive funding round budget

## Verification
- **Deliverables**: Links to completed work (code, docs, content)
- **Community Feedback**: Testimonials or usage metrics
- **Independent Review**: Third-party validation if applicable

## Budget Justification
- **Market Rate**: Comparison to similar work compensation
- **Impact Multiplier**: Value created vs. cost incurred
- **Sustainability**: Long-term benefits to the ecosystem

## Implementation
- **Payment Method**: Direct transfer or vesting schedule
- **Reporting**: Progress updates and final impact report
- **Follow-up**: Future collaboration opportunities

## Voting
- **Quorum**: 3% of circulating supply
- **Threshold**: Simple majority
- **Execution**: Grant committee multi-sig approval
```

#### Prospective Grant Template
```markdown
# Grant Proposal: [Project Name]

## Summary
Funding request for planned work to benefit the GeoDistricts ecosystem.

## Project Description
Detailed overview of proposed work and expected outcomes.

### Objectives
- **Primary Goal**: Main objective to achieve
- **Secondary Goals**: Additional benefits and impacts
- **Success Metrics**: How success will be measured

### Methodology
- **Approach**: How the work will be executed
- **Timeline**: Project phases and milestones
- **Resources Needed**: Team, tools, and external dependencies

## Team
- **Lead**: Project lead with relevant experience
- **Team Members**: Key contributors and their roles
- **Advisors**: External advisors or reviewers

## Budget Breakdown
- **Labor**: $X for team compensation
- **Tools/Infrastructure**: $Y for development resources
- **Marketing/Community**: $Z for outreach and adoption
- **Contingency**: 10% buffer for unexpected costs

## Total Budget: $XX,XXX

## Funding Request
- **Amount**: $XX,XXX in stablecoins
- **Milestone Payments**: Percentage released at completion of milestones
- **Funding Source**: Ecosystem grants budget

## Risk Assessment
- **Technical Risks**: Potential challenges and mitigation strategies
- **Timeline Risks**: Dependencies and backup plans
- **Budget Risks**: Cost overruns and contingency measures

## Success Criteria
- **Deliverables**: Specific outputs and completion standards
- **Impact Metrics**: Quantitative measures of success
- **Sustainability**: Long-term viability of project outcomes

## Voting
- **Quorum**: 3% of circulating supply
- **Threshold**: Simple majority
- **Execution**: Grant committee approval with milestone-based releases
```

### 3. Protocol Change Proposals

#### GDIP Modification Template
```markdown
# GDIP-[X] Modification: [Brief Description]

## Summary
Proposal to modify GDIP-[X] to address [issue/problem] and improve [aspect].

## Current GDIP-[X] Section
Quote the current text that needs modification:

> "Current wording from GDIP-X"

## Proposed Change
New wording or specification:

> "Proposed new wording"

## Rationale
Why this change is necessary and beneficial.

### Problem Solved
- **Issue 1**: Description and impact
- **Issue 2**: Description and impact

### Benefits
- **Benefit 1**: Quantified improvement
- **Benefit 2**: Additional advantages

## Backward Compatibility
- **Breaking Changes**: Any incompatible modifications
- **Migration Path**: How existing implementations adapt
- **Timeline**: Grace period for adoption

## Implementation
- **Reference Implementation**: Updates needed in geodistricts repo
- **Testing**: Verification procedures for compliance
- **Documentation**: Updated docs and examples

## Alternatives Considered
- **Option 1**: Description and why rejected
- **Option 2**: Description and why rejected

## Voting
- **Quorum**: 5% of circulating supply
- **Threshold**: 2/3 supermajority for protocol changes
- **Execution**: GDIP update and reference implementation merge
```

### 4. Constitutional Amendments

#### Governance Change Template
```markdown
# Constitutional Amendment: [Change Description]

## Summary
Proposal to modify the DAO constitution regarding [governance aspect].

## Current Constitution
Reference to current governance document section:

> "Current governance rule"

## Proposed Amendment
New governance rule:

> "Proposed new rule"

## Rationale
Why this constitutional change is necessary.

### Problem Addressed
- **Governance Issue**: Current limitation and its impact
- **Improvement**: How the change enhances governance

### Precedent
- **Similar DAOs**: How other DAOs handle this issue
- **Best Practices**: Governance research supporting the change

## Impact Assessment
- **Participation**: Effect on voter turnout and engagement
- **Security**: Impact on DAO security and attack resistance
- **Efficiency**: Changes to decision-making speed and quality

## Implementation Timeline
- **Immediate**: Emergency provisions during transition
- **Transition Period**: 30 days for adaptation
- **Full Implementation**: Complete adoption timeline

## Risk Mitigation
- **Unintended Consequences**: Potential negative effects and safeguards
- **Appeal Process**: How to challenge the amendment
- **Reversion Plan**: How to undo if problems arise

## Voting
- **Quorum**: 10% of circulating supply
- **Threshold**: 2/3 supermajority required
- **Execution**: Governance contract update via Timelock
```

## Proposal Submission Checklist

### Pre-Snapshot
- [ ] Forum discussion with community feedback
- [ ] Working group consultation (if applicable)
- [ ] Template compliance and completeness
- [ ] Draft review by 2+ community members

### Snapshot Submission
- [ ] Clear, descriptive title
- [ ] Complete template with all required sections
- [ ] Realistic voting parameters
- [ ] Links to forum discussion and supporting materials

### Post-Voting
- [ ] Execution monitoring and verification
- [ ] Implementation timeline adherence
- [ ] Community reporting and feedback collection
- [ ] Documentation updates for approved changes

## Support Resources

### Proposal Help
- **Template Repository**: Standardized templates in governance folder
- **Mentorship Program**: Experienced proposers available for guidance
- **Working Groups**: Domain experts for technical review

### Voting Support
- **Voting Guide**: Step-by-step voting instructions
- **Delegation Options**: How to delegate voting power
- **Analytics Tools**: Proposal impact and voting power calculators

These templates ensure consistent, comprehensive proposals that enable effective DAO governance and decision-making.