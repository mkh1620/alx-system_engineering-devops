Outage Duration: The outage spanned from 06:00 WAT to 19:40 PST following the deployment of ALX's System Engineering & DevOps project 0x19.
Impact: This incident impacted a specialized Ubuntu 14.04 container running an Apache web server tasked with hosting a WordPress site for Holberton School. Users attempting to access the site received a 500 Internal Server Error, indicating a complete failure in service delivery.
Root Cause: The problem was traced to a misconfigured file name in the site's WordPress settings, where a critical file extension was mistakenly written with an additional character.

Timeline of Events

06:00 WAT: Outage onset immediately following the deployment of the new DevOps project.
Detected at 19:20 PST: Issue first noticed by engineer Brennan when attempting to access the site.
Detection Method: The problem was identified manually by Brennan during routine project operations.
19:21 PST: Investigation began with verification of the Apache processes using ps aux, which appeared normal.
19:25 PST: Configuration files under /etc/apache2/sites-available were reviewed; no issues found.
19:30 PST: strace employed on the Apache root process revealed no abnormalities.
19:35 PST: Further strace on the www-data process uncovered an ENOENT (File not found) error linked to a .phpp file extension.
19:37 PST: Manual correction of the .phpp typo to .php in wp-settings.php.
19:40 PST: Confirmation of issue resolution through successful curl test, yielding a 200 status code.
Detailed Explanation of Cause and Resolution
Cause: An erroneous file extension within the WordPress configuration (wp-settings.php) file pointed to a non-existent PHP file, halting the WordPress engine and causing the server to return an error to all visitor requests.
Resolution: The typo was corrected manually, and the file path was restored to its correct form, thereby resuming normal operation of the WordPress site.

Corrective and Preventive Measures
Improvements Suggested:

Enhanced Pre-deployment Testing: Implement more robust testing regimes to catch such errors.
Monitoring Enhancements: Upgrade system monitoring to alert for specific HTTP error codes that indicate similar critical failures.
Actionable Tasks:

Develop and Implement a Testing Protocol: Create a checklist and automated tests that must be passed before any code moves to production.
Set Up Specific HTTP Error Alerts: Use monitoring tools to set alerts for HTTP 500 errors to catch failures immediately.
Routine Developer Training: Regular sessions on common pitfalls in code configurations, emphasizing the importance of detail-oriented programming.
Script Enhancements for Deployment Safety: Modify deployment scripts to perform automatic checks for common misconfigurations in file paths and extensions.
Automated Quick Fixes via Puppet: Deploy and maintain Puppet scripts like 0-strace_is_your_friend.pp to automatically correct predefined types of configuration errors, minimizing downtime.
