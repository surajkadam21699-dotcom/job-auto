# LinkedIn Job Automation with Claude AI

## 🎯 Overview

This project automates your job search on LinkedIn using:
- **GitHub Actions** for scheduling and orchestration
- **Claude AI** for intelligent job-resume matching
- **Google Sheets** for tracking and logging results

## ✨ Features

✅ **Automated Job Search** - Runs on schedule (weekdays at 9 AM) or manual trigger
✅ **Smart Filtering** - Removes duplicate/already-applied jobs
✅ **Claude AI Scoring** - Matches jobs against your resume (0-100 score)
✅ **Decision Gate** - Only processes jobs above score threshold (default: 70)
✅ **Auto-Apply** - Sends Easy Apply or connection requests
✅ **Results Tracking** - Logs all applications to Google Sheets
✅ **Slack Notifications** - Get notified when jobs are processed

## 🚀 Quick Setup

### 1. Clone Repository
```bash
git clone https://github.com/yourusername/job-auto.git
cd job-auto
```

### 2. Required Secrets

Add these secrets to your GitHub repository (Settings → Secrets and variables → Actions):

| Secret | Description | How to Get |
|--------|-------------|----------|
| `CLAUDE_API_KEY` | Anthropic Claude API key | [https://console.anthropic.com](https://console.anthropic.com) |
| `LINKEDIN_TOKEN` | LinkedIn OAuth access token | [LinkedIn Developer](https://www.linkedin.com/developers) |
| `GOOGLE_SHEETS_ID` | Your Google Sheet ID | Create sheet, copy ID from URL |
| `GOOGLE_SHEETS_CREDS` | Google service account JSON | [Google Cloud Console](https://console.cloud.google.com) |
| `SLACK_WEBHOOK_URL` | (Optional) Slack webhook | [Slack API](https://api.slack.com/apps) |

### 3. Configure job_config.json

Customize your job search parameters:

```json
{
  "search_parameters": {
      "keywords": ["Software Engineer", "Backend Developer"],
          "locations": ["United States", "Remote"],
              "min_salary": 100000,
                  "experience_levels": ["Mid-level", "Senior"]
                    },
                      "scoring": {
                          "job_match_threshold": 70  # Only process jobs with 70+ score
                            }
                            }
                            ```

                            ### 4. Set Workflow Schedule

                            Edit `.github/workflows/job-automation.yml`:

                            ```yaml
                            on:
                              schedule:
                                  - cron: '0 9 * * 1-5'  # 9 AM, Monday-Friday
                                    workflow_dispatch:       # Manual trigger
                                    ```

                                    ## 📊 How It Works

                                    ### 7-Step Pipeline:

                                    **1. TRIGGER** → Initialization with search parameters
                                    **2. SEARCH LINKEDIN** → Fetch job listings matching criteria
                                    **3. FILTER & DEDUPLICATE** → Remove already-applied jobs
                                    **4. ANALYZE WITH CLAUDE** → AI scores each job (0-100)
                                    **5. DECISION GATE** → Filter by score threshold
                                    **6. APPLY OR CONNECT** → Send Easy Apply/connection requests
                                    **7. LOG RESULTS** → Save to Google Sheets + artifacts

                                    ## 🔧 Usage

                                    ### Automatic Trigger
                                    Workflow runs automatically on weekday mornings (9 AM)

                                    ### Manual Trigger
                                    1. Go to Actions → LinkedIn Job Automation
                                    2. Click "Run workflow"
                                    3. (Optional) Set custom threshold (0-100)
                                    4. Click "Run workflow"

                                    ### View Results

                                    - **Google Sheets** - Open your configured sheet for tracking
                                    - **GitHub Actions** - Check workflow logs for details
                                    - **Artifacts** - Download `job-results.json` for offline analysis
                                    - **Slack** - Receive notification (if configured)

                                    ## 📝 Configuration Guide

                                    ### job_config.json Options

                                    ```json
                                    {
                                      "search_parameters": {
                                          "keywords": [],              # Job titles to search
                                              "locations": [],             # Geographic locations
                                                  "experience_levels": [],     # Entry/Mid/Senior
                                                      "employment_types": [],      # Full-time/Contract
                                                          "min_salary": 0,             # Minimum salary
                                                              "max_salary": null,          # Max salary (null = no limit)
                                                                  "date_posted_filter": "7d"   # 7d, 30d, etc.
                                                                    },
                                                                      "filtering": {
                                                                          "exclude_companies": [],     # Skip these companies
                                                                              "required_skills": [],       # Must-have skills
                                                                                  "nice_to_have_skills": []    # Bonus skills
                                                                                    },
                                                                                      "scoring": {
                                                                                          "job_match_threshold": 70,   # Minimum score to process
                                                                                              "skill_match_weight": 0.4,   # 40% weight on skills
                                                                                                  "salary_match_weight": 0.3,  # 30% weight on salary
                                                                                                      "company_fit_weight": 0.3    # 30% weight on company
                                                                                                        },
                                                                                                          "action": {
                                                                                                              "auto_apply": true,          # Auto-apply to jobs?
                                                                                                                  "apply_method": "easy_apply",# easy_apply or connect
                                                                                                                      "connection_message": "Hi!..." # DM to recruiters
                                                                                                                        }
                                                                                                                        }
                                                                                                                        ```
                                                                                                                        
                                                                                                                        ## 🔐 Security Best Practices
                                                                                                                        
                                                                                                                        ✅ **Never commit API keys** - Use GitHub Secrets
                                                                                                                        ✅ **Use service accounts** - For Google Sheets, not personal
                                                                                                                        ✅ **Rotate tokens** - Refresh LinkedIn tokens monthly
                                                                                                                        ✅ **Audit logs** - Check workflow runs for any issues
                                                                                                                        ✅ **Limit scope** - Only request needed permissions
                                                                                                                        
                                                                                                                        ## 🐛 Troubleshooting
                                                                                                                        
                                                                                                                        ### Workflow not running?
                                                                                                                        - Check Actions tab for errors
                                                                                                                        - Verify secrets are set correctly
                                                                                                                        - Confirm schedule is in correct timezone
                                                                                                                        
                                                                                                                        ### Jobs not found?
                                                                                                                        - Check LinkedIn token is valid/not expired
                                                                                                                        - Verify keywords and locations in config
                                                                                                                        - Check if job listing has Easy Apply enabled
                                                                                                                        
                                                                                                                        ### Low scores?
                                                                                                                        - Update your resume in job_config.json
                                                                                                                        - Adjust skill weights for better matching
                                                                                                                        - Lower threshold to process more jobs
                                                                                                                        
                                                                                                                        ### Google Sheets not updating?
                                                                                                                        - Verify service account has Sheets API enabled
                                                                                                                        - Check sheet ID is correct
                                                                                                                        - Confirm credentials JSON is properly formatted
                                                                                                                        
                                                                                                                        ## 📚 API References
                                                                                                                        
                                                                                                                        - [Claude API Docs](https://docs.anthropic.com)
                                                                                                                        - [LinkedIn Jobs API](https://learn.microsoft.com/en-us/linkedin/talent/)
                                                                                                                        - [Google Sheets API](https://developers.google.com/sheets)
                                                                                                                        
                                                                                                                        ## 📄 License
                                                                                                                        
                                                                                                                        MIT License - Feel free to fork and customize!
                                                                                                                        
                                                                                                                        ## 💡 Tips & Tricks
                                                                                                                        
                                                                                                                        - **Customize Claude prompt** - Edit the job analysis prompt for different job criteria
                                                                                                                        - **Weekly digest** - Add scheduled email summary of applications
                                                                                                                        - **Slack updates** - Get real-time notifications in Slack
                                                                                                                        - **Multiple profiles** - Create separate repos for different job searches
                                                                                                                        - **Resume versioning** - Update config.json as your skills change
                                                                                                                        
                                                                                                                        ## 🤝 Contributing
                                                                                                                        
                                                                                                                        Found a bug or have suggestions? Open an issue or PR!
                                                                                                                        
                                                                                                                        ## ⭐ Support
                                                                                                                        
                                                                                                                        If this helped you get a job, please star the repo! 🌟
