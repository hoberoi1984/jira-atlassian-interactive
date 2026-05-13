---
name: jira-atlassian-interactive
description: Interactive Jira issue creation with mandatory project/space selection, ticket type selection, and specialized metadata mapping for Tasks and Changes.
---

# Jira Atlassian Interactive

This skill ensures that Jira tickets are created with metadata correctly mapped to specific fields, providing a streamlined experience for both Tasks and Change Requests.

## Workflow

1.  **Intercept Issue Creation:** When a user requests to create a Jira ticket, do NOT immediately create it.

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

3.  **Prompt for Metadata (Branching Logic):**

    -   **If Type is "Task":**
        Call `ask_user` for:
        -   **Priority Selection:**
            -   Header: "Priority"
            -   Question: "What is the priority of this task?"
            -   Options: "Highest", "High", "Medium", "Low"
        -   **Additional Labels:**
            -   Header: "Labels"
            -   Type: "text"
            -   Question: "Any additional labels? (traiana and osttra are added by default)"

    -   **If Type is any "Change":**
        Call `ask_user` for:
        -   **Environment Selection:**
            -   Header: "Environment"
            -   Question: "Which environment is affected?"
            -   Options: "PROD", "QA", "UAT", "DEV"
        -   **Impact Selection:**
            -   Header: "Impact"
            -   Question: "What is the level of impact?"
            -   Options: "Minor", "Moderate", "Significant", "Extensive"
        -   **Change Reasons:**
            -   Header: "Reason"
            -   Question: "What is the reason for this change?"
            -   Options: "New Feature", "Bug Fix", "Infrastructure Update", "Maintenance"
        -   **Schedule Selection:**
            -   Header: "Schedule"
            -   Type: "text"
            -   Question: "Provide Planned start and Planned end (ISO 8601, e.g., 2024-05-20T10:00:00.000+0000) or leave blank."
        -   **Test/Backout Plans:**
            -   Header: "Plans"
            -   Type: "text"
            -   Question: "Provide the Test Plan and Backout Plan (or leave blank for auto-generation)."

4.  **Execute Creation (Field Mapping):**
    -   **Common Fields:**
        -   **Business Unit(s):** ALWAYS set **Traiana** (ID 27225) and **OSTTRA** (ID 27223) for `customfield_14039`.
        -   **Mandatory Labels:** ALWAYS apply `traiana` and `osttra`.
    
    -   **Task-Specific Fields:**
        -   **Priority:** Map selection to the standard Jira priority field.
        -   **Labels:** Merge user-provided labels with the mandatory defaults.

    -   **Change-Specific Fields:**
        -   **Urgency & Impact:** Map "Impact" selection to **Urgency** (customfield_10043) and **Impact** (customfield_10004).
        -   **Change Reason:** Map "Reason" selection to **Change reason** (customfield_10007).
        -   **Environment:** Map "Environment" selection to **Environment** (customfield_10187). Default to **QA** if unspecified.
        -   **Schedule:** Map "Planned start" to `customfield_10050` and "Planned end" to `customfield_10051`.
        -   **Intelligent Plan Generation:** If "Plans" is blank, generate them based on context.

    -   **Description:** Keep focused on technical details, excluding metadata mapped to the fields above.
