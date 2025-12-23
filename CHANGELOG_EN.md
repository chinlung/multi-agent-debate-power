# Changelog

All notable changes to the Multi-Agent Debate System Kiro Power will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-12-23

### Added ✨

#### Core System
- **Complete Multi-Agent Debate System**: Implemented five professional roles - Orchestrator, three Perspective Agents, and Critic
- **Six-Phase Debate Process**: Complete Phase 0-6 implementation including requirement analysis, solution generation, critical review, consensus checking
- **Parallel Agent Execution**: Phase 1 and Phase 3 support true parallel processing of three agents, significantly improving efficiency
- **Intelligent Angle Configuration**: Automatically selects optimal thinking perspectives based on requirement types (architecture design, feature development, performance optimization, etc.)

#### Evaluation & Consensus Mechanism
- **Quantitative Evaluation System**: Three-dimensional scoring for feasibility, benefit, and risk control (10 points each, total 30 points)
- **Intelligent Consensus Checking**: Supports both majority agreement (≥2 Agents) and clear leader (score difference ≥8) consensus modes
- **Critic Final Judgment**: When maximum rounds reached without consensus, Critic makes final decision

#### User Interaction
- **Multiple Interaction Options**: Continue debate, adopt solution, intervene adjustment, reset angles, etc.
- **Phased Decision Support**: Supports complex problems with staged debate analysis
- **Hybrid Solution Strategy**: Supports proposing hybrid solutions when each proposal has advantages

#### Auto-Trigger Mechanism
- **Intelligent Keyword Detection**: Automatically recognizes trigger words like "debate", "multi-angle analysis", "solution comparison"
- **Context Triggering**: Identifies contexts like "design system + considerations", "choose technology + evaluation criteria"
- **Parameter Support**: Supports `--max-rounds N` and `--perspectives "angle1,angle2,angle3"` parameters

#### MCP Tool Integration
- **Sequential Thinking**: Provides structured thinking and reasoning capabilities
- **Context7**: Access to latest library documentation and API references
- **Serena**: Code analysis, file operations, and project management tools

#### Output & Reporting
- **Structured Report Generation**: Includes final solution, score details, key insights, implementation recommendations
- **Complete Debate Records**: Preserves all rounds of solutions, challenges, responses, and scores
- **Collapsible Detailed Records**: Supports click-to-expand for complete debate process viewing

### Documentation & Guides 📚

#### Core Documentation
- **POWER.md**: Complete Power documentation including overview, role definitions, usage guide
- **README.md**: Detailed project introduction and usage instructions (Traditional Chinese)
- **README_EN.md**: Complete English documentation

#### Steering Guides
- **agent-definitions.md**: Complete definitions and responsibilities of five agent roles
- **complete-workflow.md**: Detailed implementation guide for six-phase debate process
- **usage-examples.md**: Rich usage examples and best practice cases
- **implementation-guide.md**: Implementation guide for using Kiro subagent and MCP tools
- **prompt-interface.md**: Keyword triggering and auto-invocation mechanism explanation
- **troubleshooting.md**: Common problem solutions and troubleshooting guide

### Technical Features 🔧

#### System Architecture
- **Kiro Subagent Integration**: Uses Kiro's native subagent system for execution
- **Parallel Processing Optimization**: Achieves true parallel execution through same function_calls block
- **Modular Design**: Clear role separation and phase division for easy maintenance and extension

#### Performance Optimization
- **Parallel Solution Generation**: Phase 1 three Agents simultaneously generate initial solutions
- **Parallel Response Processing**: Phase 3 three Agents simultaneously respond to Critic challenges
- **Intelligent Consensus Checking**: Avoids unnecessary debate rounds, improves decision efficiency

#### Scalability
- **Flexible Angle Configuration**: Supports both automatic configuration and manual specification of thinking angles
- **Adjustable Evaluation Criteria**: Supports specifying special evaluation criteria in requirement descriptions
- **Round Control**: Supports setting maximum debate rounds to adapt to different complexity requirements

### Use Cases 🎯

#### Supported Decision Types
- **Architecture Design**: System architecture selection, tech stack decisions
- **Feature Development**: Product feature planning, development strategy
- **Performance Optimization**: System bottleneck resolution, performance improvement strategies
- **Problem Resolution**: Online issue handling, technical debt resolution
- **Technology Selection**: Framework selection, tool evaluation

#### Usage Patterns
- **Quick Decision Mode**: Suitable for urgent problems, simple requirements (3-5 rounds)
- **Deep Analysis Mode**: Suitable for architecture decisions, long-term planning (6-10 rounds)
- **Exploratory Research Mode**: Suitable for new technology evaluation, innovative solutions

### Quality Assurance ✅

#### Testing & Validation
- **Angle Extraction Logic**: Complete Orchestrator result parsing and default configuration
- **Consensus Check Mechanism**: Multiple consensus determination logic and edge case handling
- **Error Handling**: Comprehensive exception handling and fallback mechanisms

#### Best Practices
- **Requirement Description Guide**: Provides good and poor requirement description examples
- **Angle Selection Strategy**: When to customize angles vs. when to let system auto-select
- **Interaction Participation Techniques**: How to effectively participate in debate process

### Installation & Deployment 📦

#### Installation Methods
- **Kiro Powers UI Installation**: Supports direct installation from GitHub
- **Local Directory Installation**: Supports local development and testing
- **MCP Server Configuration**: Automatic configuration of required MCP tools

#### System Requirements
- Kiro IDE installed and configured
- MCP servers: sequential-thinking, context7, serena
- Kiro subagent system support
- File read/write permissions

### License & Contribution 📄

- **MIT License**: Open-source friendly license terms
- **Contribution Guide**: Complete Fork, development, submission process instructions
- **Issue Reporting**: GitHub Issues and Discussions support

---

## Future Roadmap 🚀

### Planned Features

#### v1.1.0 (Expected Q1 2026)
- **Custom Evaluation Dimensions**: Support user-defined evaluation criteria and weights
- **Historical Decision Tracking**: Record and analyze historical decision effectiveness
- **Templated Requirements**: Requirement description templates for common scenarios

#### v1.2.0 (Expected Q2 2026)
- **Visual Debate Process**: Graphical display of debate flow and results
- **Decision Tree Generation**: Generate decision trees based on debate results
- **Team Collaboration Mode**: Support multi-person participation in debate process

#### v2.0.0 (Expected Q3 2026)
- **Machine Learning Optimization**: Optimize angle selection and scoring based on historical data
- **External Tool Integration**: Support more MCP tools and APIs
- **Enterprise Features**: Permission management, audit logs, compliance support

---

**Version Notes**:
- 🎯 **Major Version** (X.0.0): Significant feature changes or architectural adjustments
- ✨ **Minor Version** (X.Y.0): New feature additions
- 🐛 **Patch Version** (X.Y.Z): Bug fixes and minor improvements

**Update Frequency**:
- Major versions: Every 6-12 months
- Minor versions: Every 2-3 months
- Patch versions: Released as needed