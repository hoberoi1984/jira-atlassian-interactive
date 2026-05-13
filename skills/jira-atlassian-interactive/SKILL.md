---
name: jira-atlassian-interactive
description: Interactive Jira issue creation with mandatory project/space selection, ticket type selection, and precise mapping of change metadata to Jira custom fields.
---

# Jira Atlassian Interactive

This skill ensures that Jira tasks and changes are created with metadata correctly mapped to specific Jira fields (like Urgency, Environment, and Business Units) rather than just being appended to the description.

## Workflow

1.  **Intercept Issue Creation:** When a user requests to create a Jira task or issue, do NOT immediately create it using a default project.

2.  **Prompt for Core Ticket Details:**
    Call `ask_user` to gather:
    
    -   **Space Selection:** 
        -   Header: "Jira Space"
        -   Question: "Which Jira space/project key should this ticket be created under?"
        -   Options: "SYSLIN", "CLOUD", "APPS", "SYSWIN"
    
    -   **Issue Type:**
        -   Header: "Type"
        -   Question: "What type of ticket is this?"
        -   Options: "Task", "Normal Change", "Standard Change", "Expedited Change", "Emergency Change"

3.  **Prompt for Change-Specific Metadata (if Type is a Change):**
    If the user selects any "Change" type, proceed with:

    -   **Environment Selection:**
        -   Header: "Environment"
        -   Question: "Which environment is affected?"
        -   Options: "PROD", "QA", "UAT", "DEV"
    
    -   **Impact Selection:**
        -   Header: "Impact"
        -   Question: "What is the level of impact?"
        -   Options: "Minor", "Moderate", "Significant", "Extensive"

4.  **Prompt for Risk, Rationale, and Plans:**
    Call `ask_user` for:

    -   **Change Reasons:**
        -   Header: "Reason"
        -   Question: "What is the reason for this change?"
        -   Options: "New Feature", "Bug Fix", "Infrastructure Update", "Maintenance"
    
    -   **Test/Backout Plans:**
        -   Header: "Plans"
        -   Type: "text"
        -   Question: "Provide the Test Plan and Backout Plan (or leave blank for auto-generation)."

5.  **Execute Creation (Field Mapping):**
    -   **Urgency & Impact:** Map "Impact" selection directly to the **Urgency** (customfield_10043) and **Impact** (customfield_10004) fields.
    -   **Change Reason:** Map "Reason" selection directly to the **Change reason** (customfield_10007) field.
    -   **Environment:** Map "Environment" selection to the **Environment** (customfield_10187) field. Default to **QA** (IAT - ID 17817) if unspecified or requested.
    -   **Business Unit(s):** ALWAYS set **Traiana** (ID 27225) and **OSTTRA** (ID 27223) as the default selections for `customfield_14039`.
    -   **Intelligent Plan Generation:** If "Test Plan" or "Backout Plan" is left blank, generate them based on the context.
    -   **Description:** Keep the description focused on technical details, NOT the metadata found in the fields above.

## Mandatory Labels
Always apply `traiana` and `osttra` unless otherwise specified.
