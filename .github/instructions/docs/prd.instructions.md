--- 
applyTo: "docs/prd/**" 
---

Use this folder only for PRDs and related supporting files.  

Filename rules:
 - PRD: `prd-[feature].md`
 - Supporting files: no naming convention required

For each PRD, use this structure and sequence:

[Title: PRD - Product / Feature Name]

 - Document Information
   - Product / Feature Name:
   - Author: Assignee's username
   - Date: blank
   - Version: blank

 - Overview
   - Background: Why are we building this?
   - Objective: What problem are we solving?
   - Goals: What do we want to achieve?

 - Problem Statement
   - User pain point or business problem
   - Key users affected
   - Why it matters now

 - Functional Requirements
   - Ensure requirements address the problem statement.
   - For each user story, include:
     - Title
     - Statement: As a <type of user>, I want <goal>, so that <reason/benefit>.
     - Requirement details
     - Acceptance criteria using Given-When-Then format

 - Non-Functional Requirements
   Leave blank.

 - Dependencies & Constraints
   Explicitly state relevant limitations (e.g., desktop web only, mobile not supported).

 - Success Metrics
   Define measurable outcomes, such as:
   - Increase signup conversion to 2000 new user/month
   - Reduce drop-off rate by 10%.
   - Improve task completion time by a minimum of 30 minutes
