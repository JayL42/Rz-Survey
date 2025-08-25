Test mode: https://yoursite.com/survey.html?mode=test
Production mode: https://yoursite.com/survey.html?mode=production
Default (test): https://yoursite.com/survey.html

Visual Indicator:
The code already includes a visual indicator - when in test mode, 
you'll see an orange "TEST MODE" badge in the top-right corner of the page. 
This disappears in production mode.

Current Setting:
Right now your form is set to TEST MODE (IS_PRODUCTION = false), 
so it will submit to your webhook-test URL. When you're ready to go live, 
just change it to true!
