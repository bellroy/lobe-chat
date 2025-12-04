# LobeChat @ Bellroy

The current code on the `bellroy` branch is the one that is deployed
onto our staging and production servers.

Avoid modifying the source directly. This will make future merging in of the upstream repository more difficult. Try to make any modifications in files that are unlikely to clash with upstream changes.

## Running locally

You will need to create an `.env` file. The `.env` we currently use
in staging and production is [here](https://github.com/bellroy/puppet-control/blob/master/site-modules/profile/templates/lobechat/lobe-chat.env.erb).
Obviously there are a lot of secrets that are interpolated into the file, so
you will need to create/copy your own values for those.

You can find API tokens for the LLM providers in TeamPass.

### Tooling

The repository has been configured to include Nix setup for development and deployment. Obvs you don't need to run `nix develop` if you're cool like me and run `direnv` everywhere.

### Database Setup

LobeChat requires PostgreSQL with the pgvector extension (version 0.5.0 or later) for local development.

#### Prerequisites

1. **Install pgvector** (if not already installed or needs upgrade):

   ```bash
   brew install pgvector
   ```

2. **Create the database**:

   ```bash
   createdb lobechat
   ```

3. **Enable the vector extension**:

   ```bash
   psql -d lobechat -c "CREATE EXTENSION IF NOT EXISTS vector;"
   ```

4. **Verify pgvector version** (must be 0.5.0 or later for HNSW index support):

   ```bash
   psql -d lobechat -c "SELECT extversion FROM pg_extension WHERE extname = 'vector';"
   ```

   If the version is older than 0.5.0, upgrade pgvector:

   ```bash
   brew upgrade pgvector
   # Restart PostgreSQL, then:
   psql -d lobechat -c "ALTER EXTENSION vector UPDATE;"
   ```

The application requires the following configuration:

```bash
# LobeChat Environment Configuration

# OpenAI Configuration
OPENAI_API_KEY="abc123"

# Anthropic Configuration
ANTHROPIC_API_KEY="abc123"

# Google Configuration
GOOGLE_API_KEY="abc123"

# Development Mode (disable authentication)
NEXT_PUBLIC_ENABLE_NEXT_AUTH=0
NEXT_PUBLIC_ENABLE_BETTER_AUTH=0
ENABLE_AUTH_PROTECTION=0
ENABLE_MOCK_DEV_USER=1
MOCK_DEV_USER_ID=dev-user-123

# Server database configuration (uses local PostgreSQL)
DATABASE_DRIVER=node
DATABASE_URL=postgresql://postgres:your_password@localhost:5432/lobechat
KEY_VAULTS_SECRET=abc123

# S3 Configuration
S3_ENDPOINT=https://s3.ap-southeast-2.amazonaws.com
S3_BUCKET=lobechat-development
S3_REGION=ap-southeast-2
S3_PUBLIC_DOMAIN=https://s3.amazonaws.com/not-a-real-s3-bucket
S3_ACCESS_KEY_ID=abc123
S3_SECRET_ACCESS_KEY=abc123
S3_SET_ACL=0

# Application Configuration
APP_URL=http://localhost:3010

# Explicitly disable certain providers
ENABLED_FAL=0
ENABLED_OLLAMA=0
ENABLED_QWEN=0

# Feature Flags
FEATURE_FLAGS=-check_updates,-openai_api_key,-welcome_suggest,-language_model_settings,+group_chat,-market
```

### Installing dependencies

```bash
nix develop
pnpm install
```

### Running the application

First, run database migrations:

```bash
nix develop
pnpm run db:migrate
```

Then start the development server:

```bash
pnpm dev
```
