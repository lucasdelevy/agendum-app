# Agendum App

A professional Vue.js calendar application with authentication and weekly view.

## Features

- **Authentication System** - Login/logout with session persistence
- **Weekly Calendar View** - Navigate through weeks with arrow controls
- **Hourly Grid** - 24-hour time slots with current time indicator
- **Current Day Highlighting** - Visual indication of today's column
- **Professional UI** - Sidebar navigation with hamburger menu
- **Responsive Design** - Clean, modern interface

## Getting Started

### Prerequisites

- Node.js (version 18 or higher)
- npm

### Installation

1. Clone or navigate to the project directory:
   ```bash
   cd agendum-app
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

4. Open your browser and go to `http://localhost:5173`

### Login

- Enter any email and password to log in
- Session persists across browser refreshes
- Use the logout button in the header to sign out

## Build for Production

```bash
npm run build
```

The built files will be in the `dist/` directory.

## CI/CD: Automatic AWS Deployment + Private URL

This repo includes a GitHub Actions workflow at `.github/workflows/deploy.yml` that:

- **On pull requests to `main`**: runs a production build check
- **On push to `main`**: builds and deploys the app to your AWS account automatically

Deployment uses:

- **S3** (private bucket for static files)
- **CloudFront** (HTTPS delivery)
- **SSM Parameter Store** (stores deployment metadata, including the private path)

The workflow publishes a stable private URL in the run summary, with an unguessable path:

`https://<cloudfront-domain>/<private-path>/`

### One-time setup: AWS OIDC for GitHub Actions

GitHub Actions authenticates to AWS using OIDC (no long-lived AWS keys needed).

1. Ensure your AWS CLI is configured with credentials that can create IAM/CloudFormation resources.
2. Run:

```bash
./setup-aws-oidc.sh --region us-east-1
```

This creates:
- A GitHub OIDC provider in your AWS account (or reuses an existing one)
- A deploy role scoped to this repository/branch

### Add GitHub repository secrets

Go to: `Settings -> Secrets and variables -> Actions`

Add these **secrets**:

| Secret | Value |
|--------|-------|
| `AWS_ROLE_ARN` | Role ARN printed by `setup-aws-oidc.sh` |
| `AWS_REGION` | Deployment region (for example `us-east-1`) |

Optional **repository variable**:

| Variable | Value |
|----------|-------|
| `AWS_STATIC_BUCKET` | Existing S3 bucket name to use for hosting |

If `AWS_STATIC_BUCKET` is not set, the workflow creates/uses:

`agendum-app-<account-id>-<region>`

### Accessing your deployed private URL

After each deploy run, open the GitHub Actions run and check the **Summary** section for:

- Private app URL
- CloudFront distribution ID
- S3 bucket name

You can also fetch the stored values from AWS Systems Manager Parameter Store:

- `/agendum-app/private_path`
- `/agendum-app/cloudfront_distribution_id`
- `/agendum-app/cloudfront_domain_name`

## Project Structure

```
src/
├── components/
│   ├── AppHeader.vue      # Main header with menu and logout
│   ├── LoginPage.vue      # Authentication form
│   ├── Sidebar.vue        # Navigation menu
│   └── WeeklyCalendar.vue # Main calendar component
├── App.vue                # Root component with auth logic
└── main.js                # Application entry point
```