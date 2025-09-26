# sinalogy.io

## Git Repository Access Tokens - Credentials Required for Push/Pull Operations

This guide explains the credentials required when creating an access token for pushing to and pulling from a Git repository, particularly on platforms like GitHub, GitLab, and Bitbucket.

### Types of Access Tokens

#### 1. Personal Access Tokens (PATs)
Personal Access Tokens are the most common way to authenticate Git operations when using HTTPS.

**Required Credentials:**
- **Username**: Your Git platform username (GitHub/GitLab/Bitbucket username)
- **Email**: Your registered email address (for commit attribution)
- **Token**: The generated access token (replaces password)

**Required Permissions/Scopes for Push/Pull:**
- `repo` (GitHub) or `read_repository`/`write_repository` (GitLab) - Full repository access
- `read:org` - If working with organization repositories
- For push operations: `repo` or `write_repository` scope is mandatory
- For pull operations: `repo` or `read_repository` scope is sufficient

#### 2. Deploy Keys
Deploy keys provide repository-specific access.

**Required Credentials:**
- **SSH Key Pair**: Public and private SSH keys
- **Repository Access**: Keys must be added to specific repositories
- **Read/Write Permissions**: Must be configured when adding the deploy key

#### 3. OAuth Apps
For applications accessing repositories on behalf of users.

**Required Credentials:**
- **Client ID**: Application identifier
- **Client Secret**: Application secret
- **OAuth Token**: User-authorized token
- **Redirect URI**: Configured callback URL

### Step-by-Step Guide: Creating a Personal Access Token

#### GitHub
1. **Login Requirements:**
   - GitHub username/email
   - GitHub password or existing authentication

2. **Token Creation:**
   - Go to Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Click "Generate new token"
   - Select scopes: `repo` for full repository access
   - Set expiration (recommended: 90 days max)

3. **Required Information:**
   - **Token name**: Descriptive name for the token
   - **Expiration**: Token validity period
   - **Scopes**: Minimum `repo` scope for push/pull operations

#### GitLab
1. **Login Requirements:**
   - GitLab username/email
   - GitLab password or existing authentication

2. **Token Creation:**
   - Go to User Settings → Access Tokens
   - Fill in token details
   - Select scopes: `read_repository` (pull) + `write_repository` (push)

3. **Required Information:**
   - **Token name**: Descriptive identifier
   - **Expiration date**: Token validity
   - **Scopes**: `api`, `read_repository`, `write_repository`

#### Bitbucket
1. **Login Requirements:**
   - Bitbucket username/email
   - Bitbucket password or existing authentication

2. **Token Creation:**
   - Go to Personal settings → App passwords
   - Create app password with repository permissions

3. **Required Information:**
   - **App password name**: Descriptive identifier
   - **Permissions**: `Repositories: Read` and `Repositories: Write`

### Using Access Tokens

#### HTTPS Authentication
```bash
# Using token as password (username: your-username, password: your-token)
git clone https://your-username:your-token@github.com/username/repository.git

# Or configure git credentials
git config --global credential.helper store
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

#### SSH Authentication
```bash
# Generate SSH key pair
ssh-keygen -t ed25519 -C "your-email@example.com"

# Add SSH key to ssh-agent
ssh-add ~/.ssh/id_ed25519

# Add public key to your Git platform account
cat ~/.ssh/id_ed25519.pub
```

### Security Best Practices

1. **Token Security:**
   - Never commit tokens to repositories
   - Use environment variables: `GITHUB_TOKEN`, `GITLAB_TOKEN`
   - Set appropriate expiration dates
   - Use minimal required scopes

2. **Credential Storage:**
   - Use credential managers (Git Credential Manager, keychain)
   - Avoid storing tokens in plain text
   - Use SSH keys when possible

3. **Token Management:**
   - Regularly rotate tokens
   - Revoke unused tokens
   - Monitor token usage in platform audit logs

### Environment Variables Setup

```bash
# Set environment variables for automation
export GITHUB_TOKEN="your-github-token"
export GITLAB_TOKEN="your-gitlab-token"

# Use in Git operations
git clone https://${GITHUB_TOKEN}@github.com/username/repo.git
```

### Troubleshooting Common Issues

#### Authentication Failed
- **Check token validity**: Ensure token hasn't expired
- **Verify scopes**: Confirm token has required permissions
- **Check repository access**: Ensure you have access to the repository

#### Permission Denied
- **Verify token scopes**: Must include `repo` or `write_repository` for push
- **Check repository settings**: Confirm repository allows token access
- **Organization policies**: Check if organization restricts token access

#### Token Not Working
- **Regenerate token**: Create new token if current one is compromised
- **Clear cached credentials**: Remove old credentials from system
- **Check token format**: Ensure no extra characters or spaces

### Minimum Required Credentials Summary

For **Push and Pull operations**, you need:

1. **Authentication Method**: Choose one
   - Personal Access Token with `repo` scope
   - SSH Key pair added to your account
   - OAuth token with repository permissions

2. **Git Configuration**:
   - `user.name`: Your name for commits
   - `user.email`: Your email for commits
   - Credential helper configuration (recommended)

3. **Repository Access**:
   - Read permissions (for pull/clone)
   - Write permissions (for push)
   - Repository must exist and be accessible to your account

The most common and recommended approach is using a **Personal Access Token with repo scope** for HTTPS operations or **SSH keys** for SSH operations.