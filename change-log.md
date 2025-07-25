# Command: change-log

Creates a change log entry for the feature that was just completed in the current chat session.

## Usage
/change-log

## Example
/change-log

## Implementation

1. First, run `date "+%Y-%m-%d"` to get the current date
2. Use that exact date output in the filename (e.g., if the date command returns "2025-07-09", use that in the filename)
3. Create a new entry markdown file in the /docs/change_log/pending folder to record the following information for the feature that was just completed (everything in the current chat):  

The .md file should have the following format and information:  
**File Name: YYYY-MM-DD_Feature_Name** (IMPORTANT: First run `date "+%Y-%m-%d"` to get the current date and use that exact date in the filename - don't type the date manually)

**Title**: 
* A short, action-oriented summary of the feature using sentence case

**Date**:
* the date and time in MDT that the feature was finished

**Requested by**:
* the user who requested it

**Type:**
* Bug fix, feature request, major project, optimization

**Impact**  
A brief Impact: section could be beneficial, outlining who or what this feature affects (e.g., "All users," "Administrators only," "Performance improvement").  

**Overview**:
* A detailed but user-friendly description of the feature. Focus on **why** this feature was added, what it enables, and which user flows it touches.

**Technical Details**: 
* File names and paths  
* Key functions or components added/modified  
* Schema or model updates  
* New environment variables or config flags  
* Relevant migrations, hooks, or side effects

**Testing Notes**
* Any special testing steps, edge cases found, or notes for QA.  
* Suggested unit tests to cover the feature

**Documentation**
* Reference any .md file that documents the feature  
* Reference or link to Jira ticket if given

## Files to Reference
- `/docs/change_log/pending` - Directory where change logs are stored

## Notes
- This command should analyze the current chat session to extract feature details
- Automatically generate the file name with current date
- Create the file in the `/docs/change_log/pending` directory

## Mark To Dos Done
- check /docs/to_do
_ if there is a to_do item for this feature mark it completed by putting in /docs/to_do/completed
- if you are working on a jira ticket mark it as done

## Check in
Check file changes in git associated with this fix and push to origin 