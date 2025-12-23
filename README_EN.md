# Multi-Agent Debate System Kiro Power

[![Kiro Power](https://img.shields.io/badge/Kiro-Power-blue)](https://kiro.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/Version-1.0.0-green.svg)](https://github.com/chinlung/multi-agent-debate-power)

An innovative decision analysis tool that conducts multi-round debates through a coordinator, three perspective agents, and a critic to perform in-depth analysis of complex requirements and generate optimal practical solutions. Integrates sequential-thinking, context7, and serena MCP tools with support for parallel agent execution and intelligent consensus checking.

## 🚀 Key Features

- **Five Professional Roles**: Orchestrator, three Perspective Agents, Critic
- **Parallel Agent Execution**: Phase 1 and Phase 3 support parallel processing of three agents, significantly improving efficiency
- **Intelligent Angle Configuration**: Automatically selects the most suitable thinking perspectives based on requirement types
- **Multi-Round Debate Process**: Complete 6-phase implementation including consensus checking and user interaction
- **Quantitative Evaluation System**: Three-dimensional scoring for feasibility, benefit, and risk control (total 30 points)
- **Structured Debate**: Achieve consensus or final critic judgment through multi-round criticism and improvement
- **Complete Record Tracking**: Preserve complete debate process and decision rationale
- **MCP Tool Integration**: Integrates sequential-thinking, context7, and serena tools
- **Kiro Subagent Support**: Uses Kiro's native subagent system for execution
- **Keyword Auto-Trigger**: Intelligently detects requirements and automatically initiates debate analysis
- **User Interaction Mechanism**: Supports intervention adjustments, angle reset, and phased decision-making

## 🎯 Use Cases

| Scenario Type | Typical Applications | Expected Benefits |
|---------------|---------------------|-------------------|
| **Architecture Design** | System architecture selection, tech stack decisions | Balance performance, maintainability, scalability |
| **Feature Development** | Product feature planning, development strategy | Consider delivery speed, quality, user experience |
| **Performance Optimization** | System bottleneck resolution, performance improvement | Multi-angle optimization strategy comparison |
| **Problem Resolution** | Online issue handling, technical debt | Balance quick fixes vs. fundamental solutions |
| **Technology Selection** | Framework selection, tool evaluation | Assess stability, innovation, applicability |

## 🛠️ Installation

### Via Kiro Powers UI (Recommended)

1. Open the Powers panel in Kiro IDE
2. Click "Available Powers" → "Manage Repos" → "Add Repository"
3. Select "Import power from GitHub" and enter:
   ```
   https://github.com/chinlung/multi-agent-debate-power
   ```
4. Find "Multi-Agent Debate System" in the available Powers list and install

### Local Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/chinlung/multi-agent-debate-power.git
   ```

2. Add local directory in Kiro Powers UI:
   - Open Powers panel in Kiro IDE
   - Click "Available Powers" → "Manage Repos" → "Add Repository"
   - Select "Local Directory"
   - Choose path: `/path/to/multi-agent-debate-power`

## 📖 Usage

### Quick Start

1. **After installing the Power**, activate in Kiro:
   ```
   Call action "activate" with powerName="multi-agent-debate"
   ```

2. **Basic Usage Examples**:
   ```bash
   # Auto-trigger (recommended)
   "Please conduct multi-angle debate analysis for e-commerce recommendation system"
   
   # Parameterized usage
   "Debate analysis of microservice architecture, max 5 rounds, from performance, maintainability, cost perspectives"
   
   # Manual trigger
   /debate I want to add product recommendation feature to e-commerce website
   ```

3. **Advanced Usage Examples**:
   ```bash
   # Specify maximum rounds
   /debate Refactor user authentication system --max-rounds 5
   
   # Custom thinking perspectives
   /debate Design caching strategy --perspectives "Performance Priority,Cost Priority,Simplicity Priority"
   ```

### Detailed Documentation

After Power installation, you can access complete documentation through:

- **Main Documentation**: `Call action "activate" with powerName="multi-agent-debate"`
- **Agent Definitions**: `Call action "readSteering" with powerName="multi-agent-debate", steeringFile="agent-definitions.md"`
- **Usage Examples**: `Call action "readSteering" with powerName="multi-agent-debate", steeringFile="usage-examples.md"`
- **Implementation Guide**: `Call action "readSteering" with powerName="multi-agent-debate", steeringFile="implementation-guide.md"`
- **Complete Workflow**: `Call action "readSteering" with powerName="multi-agent-debate", steeringFile="complete-workflow.md"`
- **Keyword Triggers**: `Call action "readSteering" with powerName="multi-agent-debate", steeringFile="prompt-interface.md"`
- **Troubleshooting**: `Call action "readSteering" with powerName="multi-agent-debate", steeringFile="troubleshooting.md"`

## 🏗️ System Architecture

```
Requirement Input → Orchestrator → Angle Configuration
    ↓
Phase 1: Perspective A/B/C → ⚡ Parallel Solution Generation
    ↓
Phase 2: Critic → Review & Challenge → Quantitative Scoring
    ↓
Phase 3: Agent Response & Revision → ⚡ Parallel Processing
    ↓
Phase 4: Consensus Check → User Interaction → Decision Choice
    ↓
Phase 5: Final Solution → Structured Report
```

### Complete Debate Process (6 Phases)

- **Phase 0**: Requirement analysis and angle configuration
- **Phase 1**: Initial solution generation (⚡ Parallel execution)
- **Phase 2**: Critical review and quantitative scoring
- **Phase 3**: Rebuttal and revision (⚡ Parallel execution)
- **Phase 4**: Consensus check and score analysis
- **Phase 5**: User interaction and selection
- **Phase 6**: Final output and report generation

### Core Roles

- **🎯 Orchestrator**: Analyzes requirements and determines thinking angles for three agents
- **🅰️ Perspective Agent A**: Proposes solutions from specified angle
- **🅱️ Perspective Agent B**: Proposes alternative solutions from different angle
- **🅲 Perspective Agent C**: Provides third perspective solutions
- **🔍 Critic**: Objectively reviews all solutions, challenges them, and provides quantitative scoring

### Parallel Execution Mechanism

The system supports parallel processing at key phases:
- **Phase 1**: Three Perspective Agents simultaneously generate initial solutions
- **Phase 3**: Three Agents simultaneously respond to Critic's challenges
- Achieved through Kiro's `invokeSubAgent` in the same function_calls block for true parallel execution

## 📊 Scoring Criteria

The system uses three dimensions for quantitative evaluation (10 points each, total 30 points):

| Dimension | Scoring Criteria | Weight |
|-----------|------------------|--------|
| **Feasibility** | Technical achievability, resource availability, timeline reasonableness | 33.3% |
| **Benefit** | Problem-solving degree, positive value brought, ROI | 33.3% |
| **Risk Control** | Risk identification completeness, mitigation measure reliability, failure impact scope | 33.3% |

### Consensus Check Mechanism

- **Consensus Achieved**: ≥2 Agents agree on a solution
- **Clear Leader**: Score difference between highest and second highest ≥8 points
- **Final Judgment**: Critic makes final decision when maximum rounds reached

## 🎯 Usage Examples

### Example 1: Architecture Design Decision

```bash
/debate Design backend architecture for e-commerce platform supporting millions of users
```

**Expected Angle Configuration**:
- Agent A (Performance Priority): Microservices + Redis Cluster + CDN
- Agent B (Maintainability Priority): Modular Monolith + Clear Layering
- Agent C (Scalability Priority): Event-Driven Architecture + Horizontal Scaling

### Example 2: Technology Selection

```bash
/debate Choose frontend framework for new project --perspectives "React Ecosystem,Vue Ecosystem,Native Solution"
```

### Example 3: Performance Optimization

```bash
/debate Solve database query performance issues --max-rounds 6
```

## 📋 Output Format

The system generates structured analysis reports:

```markdown
# 🎯 Final Solution

## Adopted Solution: [Solution Name]
**Source**: Agent [X] ([Consensus Status])
**Total Score**: X/30

[Complete solution content]

## 📊 Score Details

| Agent | Feasibility | Benefit | Risk Control | Total |
|-------|-------------|---------|--------------|-------|
| A     | X/10        | X/10    | X/10         | X/30  |
| B     | X/10        | X/10    | X/10         | X/30  |
| C     | X/10        | X/10    | X/10         | X/30  |

## 🔍 Key Insights
[Main findings and learnings from the debate process]

## 📋 Implementation Recommendations
[Specific implementation steps and considerations]

## 📜 Debate Process Summary
**Total Rounds**: X rounds
**Consensus Achieved**: [Yes/No]
**Execution Mode**: Parallel Multi-Agent Debate System

<details>
<summary>📜 Complete Debate Record (Click to expand)</summary>
[Complete debate record including solutions, challenges, and responses from each round]
</details>
```

## 💡 Best Practices

### Requirement Description Techniques

✅ **Good Requirement Description**:
```
Requirement: Add multi-tenant support to SaaS platform

Background:
- Current system: Django + PostgreSQL
- User scale: 100 enterprise customers, 50-200 users each
- Data isolation requirement: Strict isolation, GDPR compliant

Technical Constraints:
- Cannot affect existing functionality
- Database migration time <4 hours
- Performance cannot degrade >10%

Business Goals:
- Support 1000+ enterprise customers
- Reduce deployment and maintenance costs
- Improve data security

Success Criteria:
- Complete data isolation
- Maintain query performance
- Reduce deployment complexity
```

❌ **Poor Description**:
```
Requirement: System is too slow, needs optimization
```

### Angle Selection Strategy

**When to customize angles**:
```bash
# Specific business constraints
/debate Design payment system --perspectives "Security Priority,Compliance Priority,User Experience Priority"

# Clear technical preferences
/debate Choose frontend framework --perspectives "React Ecosystem,Vue Ecosystem,Native JavaScript"
```

**When to let system auto-select**:
```bash
# General technical problems
/debate Optimize database query performance

# Exploratory requirements
/debate Improve user retention rate
```

### Interaction Participation Strategy

- **Timely Intervention**: Adjust or add conditions when debate goes off track
- **Stay Open**: Trust the debate process, don't conclude too early
- **Provide Feedback**: Share your views and preferences at the end of each round
- **Record Insights**: Note new ideas and insights generated during debate

### Usage Patterns

#### Pattern 1: Quick Decision
```bash
/debate Fix login issue --max-rounds 3
```
Suitable for urgent problems, simple requirements

#### Pattern 2: Deep Analysis
```bash
/debate Design microservice architecture --max-rounds 8
```
Suitable for architecture decisions, long-term planning

#### Pattern 3: Exploratory Research
```bash
/debate Introduce AI features to enhance product competitiveness --perspectives "Technical Feasibility,Business Value,User Acceptance"
```
Suitable for new technology evaluation, innovative solutions

## 🐛 Troubleshooting

### Common Issues

**Issue**: Agents propose overly similar solutions
**Solution**:
- Redescribe requirements with more background and constraints
- Use `--perspectives` parameter to specify clearer angles
- Choose "Reset Angles" option to reconfigure

**Issue**: Debate reaches deadlock, cannot achieve consensus
**Solution**:
- Intervene in debate, provide additional evaluation criteria or constraints
- Let Critic make final judgment
- Consider hybrid solutions combining advantages of each proposal

**Issue**: Parallel execution fails, agents execute sequentially
**Solution**:
- Ensure multiple invokeSubAgent calls are in the same function_calls block
- Check if Kiro subagent system is functioning properly
- Review execution logs to confirm parallel status

**Issue**: Scoring results are unreasonable
**Solution**:
- Check Critic's scoring logic and criteria
- Provide clearer evaluation dimension descriptions
- Request re-scoring with explanations

For detailed troubleshooting guide, see `troubleshooting.md` after installing the Power.

## 🔧 Custom Configuration

### Adjusting Evaluation Criteria

You can specify special evaluation criteria in requirement descriptions:
```
Requirement: Design caching strategy
Special requirement: Particularly emphasize cost control, performance can be moderately sacrificed
```

### Phased Decision Making

For complex problems, conduct debates in phases:
```
Phase 1: Determine overall architecture direction
Phase 2: Refine specific implementation plans
```

### Hybrid Solution Strategy

When each solution has advantages, request agents to propose hybrid solutions:
```
Intervention instruction: Please combine Agent A's caching strategy with Agent B's consistency guarantee
```

### Effectiveness Evaluation Metrics

**Debate Quality Indicators**:
- Solution diversity: Degree of difference between three solutions
- Challenge depth: Quality of questions raised by Critic
- Response quality: Completeness of Agent responses to challenges
- Consensus formation: Number of rounds and process to achieve consensus

**Decision Quality Indicators**:
- Feasibility verification: Actual executability of final solution
- Risk identification: Whether risks are adequately identified and mitigated
- Benefit realization: Whether actual effects match expectations
- Learning value: Insights and knowledge generated during process

## 🤝 Contributing

Welcome to submit Issues and Pull Requests!

1. Fork this repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Kiro IDE](https://kiro.dev) - Providing powerful Power system
- Inspired by multi-agent systems and dialectical thinking research
- All contributors and users for their feedback

## 📞 Support

- 🐛 [Report Issues](https://github.com/chinlung/multi-agent-debate-power/issues)
- 💬 [Discussions](https://github.com/chinlung/multi-agent-debate-power/discussions)
- 📧 Contact Author: scl@hanchih.com

---

**Let the Multi-Agent Debate System help you make better technical decisions!** 🚀