# Returnalyze Survey Form
Dynamic survey system that adapts to ANY question structure without code changes.

## URLs
Test mode: https://jayl42.github.io/Rz-Survey/index.html?mode=test
Production mode: https://jayl42.github.io/Rz-Survey/index.html?mode=production
Default (production): https://jayl42.github.io/Rz-Survey/index.html

## Visual Indicators
- TEST MODE: Orange badge in top-right corner
- Production: No badge
- Progress bar shows completion percentage based on required fields

## Current Configuration
Default mode: PRODUCTION (IS_PRODUCTION = true on line ~380)
To switch to test, change to false or use ?mode=test in URL

## Webhook URLs (Lines 380-384)
test: 'https://jayl42.app.n8n.cloud/webhook-test/bce6775a-1456-41a6-82c4-e70749270256'
production: 'https://jayl42.app.n8n.cloud/webhook/bce6775a-1456-41a6-82c4-e70749270256'

## Form Structure Requirements

### Required Fields
Must include 'required' attribute and asterisk (*) in label:
- Add: required attribute to input/select/textarea
- Add: <span class="required">*</span> in label

### Conditional Fields (Show/Hide Logic)
For dropdown-triggered fields:
1. Add onchange="toggleYourFunction()" to select element
2. Create conditional div with class="field conditional-field" and unique ID
3. Add JavaScript function to handle show/hide logic (see lines 395-430)

Current conditional fields:
- Q1 "Other" → Shows text field when "Other (Please Specify)" selected
- Q4 "Rarely implement" → Shows explanation field
- Q9 Department "Other" → Shows text field

### Field Naming Convention
Use descriptive 'name' attributes - these become Jira ticket field labels:
- Good: name="Your department or function"
- Bad: name="q9"

### Adding New Questions
Simply add new field blocks following the standard div structure with class="field"
Include label with for attribute, input/select/textarea with matching id and descriptive name
Add required attribute and asterisk span only if field is mandatory

### Modifying Existing Questions
1. Change question text in both label AND name attribute
2. Update option values in dropdowns
3. No n8n workflow changes needed - it auto-adapts

## Logo Configuration (Line 214)
Current: ./returnalyze_white.png (white logo in repo)
CSS filter removed since using white logo

## Style Customization (Lines 9-17)
CSS variables for brand colors:
- --primary: #4f46e5 (purple)
- --primary-hover: #4338ca
- --error: #ef4444 (red for required fields)
- --success: #10b981 (green for confirmation)

## Complete End-to-End Flow

### System Architecture
Survey Form (GitHub Pages) → n8n Webhook (POST) → Process & Label (Code Node) → 
Create Jira Ticket (CFR Project) → Notify Slack (#cx-feedback) → Handle Errors

### n8n Workflow Components
1. Webhook Node: Receives POST data from form
2. Code Node: Processes responses, creates labels, formats for Jira
3. Jira Node: Creates ticket in CFR project as "Survey Response" type
4. Slack Node: Posts notification to #cx-feedback channel
5. Error Handler: Catches and logs any failures

### Data Processing Features
The n8n workflow automatically:
- Accepts any JSON structure (no modification needed for new questions)
- Creates descriptive Jira ticket with all responses
- Auto-generates labels based on content keywords
- Adds "TEST -" prefix for test submissions
- Detects urgency keywords and sets priority
- Extracts name/company for ticket summary
- Preserves all metadata for debugging

### Auto-Labeling System
Automatically detects and labels:
- Survey type (returns-challenge, customer-feedback, general-survey)
- Month labels (survey-2025-08)
- System mentions (sap, salesforce, oracle, excel, etc.)
- Issue indicators (timing, manual-process, cost, integration)
- Response patterns (q1-answer, q2-answer format)
- Test submissions (q6-test label)
- Urgency flags (urgent-flag for critical keywords)

### Testing Checklist
□ Submit test form with ?mode=test
□ Verify "TEST -" prefix appears in Jira title
□ Check all conditional fields trigger correctly
□ Confirm required field validation works
□ Verify progress bar updates with field completion
□ Check Jira ticket created in CFR project
□ Verify Slack notification received in #cx-feedback
□ Confirm labels are properly generated

### Maintenance Notes
To create new survey:
1. Duplicate index.html
2. Modify questions (divs with class="field")
3. Update conditional logic functions if needed
4. No backend changes required - workflow auto-adapts

The system auto-detects:
- Question count
- Field types
- Response patterns
- Keywords for labeling
- Priority indicators

### Key Features
- Dynamic form that adapts to ANY structure
- Test/Production mode toggle with visual indicator
- "TEST -" prefix for test submissions
- Smart labeling with keyword detection
- Clean Jira tickets (no object/metadata pollution)
- Conditional fields for "Other" options
- Progress bar for user experience
- White Returnalyze logo
- Name capture for better summaries
- Auto-detection of urgency/priority
- Metadata preservation for debugging

### Support
For issues or modifications beyond survey questions:
- Form issues: Update index.html
- Webhook issues: Check n8n webhook node (must be POST)
- Processing issues: Review Code node in n8n
- Jira issues: Verify CFR project permissions
- Slack issues: Check bot integration in #cx-feedback