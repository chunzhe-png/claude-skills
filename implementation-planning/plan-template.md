# [Feature/Fix Name] Implementation Plan

## Jira Ticket Information

**IMPORTANT**: Before starting any implementation, you MUST provide the Jira ticket number.

- **Jira Ticket**: GRE-XXXX
- **Ticket Title**: [Brief title from Jira]
- **Ticket URL**: [Link to Jira ticket]

**Branch Naming**: `feature/GRE-XXXX-brief-description` or `bugfix/GRE-XXXX-brief-description`

---

## Executive Summary

**Key Points:**
- **Problem**: [Brief description of the issue/requirement]
- **Solution**: [High-level approach to solving it]
- **Impact**: [Expected business/technical impact]
- **Risk Level**: [LOW/MEDIUM/HIGH with brief justification]

---

## Problem Statement / Current State Analysis

### Current System
- **Existing Functionality**: [What currently exists]
- **Current Architecture**: [Relevant technical components]
- **User Experience**: [How users currently interact]
- **Data Flow**: [How data moves through the system]

### Pain Points
- [Pain point 1 with impact]
- [Pain point 2 with impact]

### What's Missing
- [ ] **[Missing Component 1]**: [Description]
- [ ] **[Missing Component 2]**: [Description]

### What Already Exists
- [x] **[Existing Component 1]**: [How it can be reused]
- [x] **[Existing Component 2]**: [How it can be extended]

---

## User Story & Requirements

**As a** [User Type]
**When** [Situation/Context]
**I want** [Desired capability]
**So that** [Business value/outcome]

### Acceptance Criteria
- [ ] [Specific, measurable requirement 1]
- [ ] [Specific, measurable requirement 2]
- [ ] [Performance requirement if applicable]
- [ ] [Security requirement if applicable]

---

## Implementation Plan

### Approach Overview

**Key Design Decisions:**
- **[Decision 1]**: [Rationale]
- **[Decision 2]**: [Rationale]

### Phase 1: [Phase Name]

**Objective**: [What this phase accomplishes]

**Tasks:**
- [ ] [Specific task 1]
- [ ] [Specific task 2]

**Files Modified/Created:**
- `path/to/file1.py` - [Description of changes]
- `path/to/file2.py` - [Description of changes]

### Phase 2: [Phase Name]

**Objective**: [What this phase accomplishes]

**Tasks:**
- [ ] [Specific task 1]
- [ ] [Specific task 2]

---

## Technical Implementation Details

### Database Changes (if applicable)

```sql
-- Migration description
-- Include schema changes, new tables, field additions
```

**Migration Strategy:**
- [How to handle existing data]
- [Backward compatibility considerations]
- [Rollback plan]

### Code Changes

**File**: `path/to/main/file.py`
```python
# Key code changes with comments
def new_function():
    """Description of what this function does"""
    pass
```

**Integration Points:**
- [How this integrates with existing systems]
- [Dependencies on other components]

---

## Testing Strategy

### Unit Tests
- [ ] [Test case 1: Description]
- [ ] [Test case 2: Description]

### Integration Tests
- [ ] [Integration scenario 1]
- [ ] [Integration scenario 2]

### Manual Testing Scenarios

1. **[Test Scenario 1]**:
   - Steps: [Step-by-step instructions]
   - Expected Result: [What should happen]

### Edge Cases & Error Scenarios
- [ ] [Edge case 1 and expected handling]
- [ ] [Error scenario 1 and expected handling]

---

## Risk Assessment & Mitigation

### Technical Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| [Risk 1] | LOW/MED/HIGH | LOW/MED/HIGH | [How to prevent/handle] |
| [Risk 2] | LOW/MED/HIGH | LOW/MED/HIGH | [How to prevent/handle] |

### Rollback Plan
- **Rollback Trigger**: [Conditions requiring rollback]
- **Rollback Steps**: [Specific steps to revert]
- **Data Recovery**: [How to handle data if rollback needed]

---

## Success Criteria & Metrics

### Immediate Success (Week 1)
- [ ] [Measurable criterion 1]
- [ ] [Measurable criterion 2]

### Short-term Metrics (Month 1)
- [Quantifiable metric with target]

---

## Files to be Created/Modified

### New Files
1. `path/to/new/file1.py` - [Purpose]
2. `path/to/new/file2.py` - [Purpose]

### Modified Files
1. `path/to/existing/file1.py` - [Description of changes]
2. `path/to/existing/file2.py` - [Description of changes]

---

## Git Workflow

### Branch Management
- **Branch Name**: `feature/GRE-XXXX-brief-description`
- **Base Branch**: `master`
- **Creation**: `git checkout -b feature/GRE-XXXX-brief-description`

### Commit Standards
- `feat(emissions): GRE-XXXX - add carbon footprint calculation`
- `fix(api): GRE-XXXX - resolve emission factor validation`
- `refactor(models): GRE-XXXX - optimize database queries`

### Pull Request Requirements
- **Title**: `GRE-XXXX: [Feature/Fix Name]`
- **Description**: Reference Jira ticket, include acceptance criteria
- **Labels**: Add appropriate labels including ticket number

---

## Definition of Done

- [ ] All code changes implemented and reviewed
- [ ] All tests passing (unit, integration, manual)
- [ ] Documentation updated
- [ ] Stakeholder approval received
- [ ] Production deployment completed
- [ ] Success criteria met

---

*Template Version: 1.0*
