# LobeChat @ Bellroy

The current code on the `main` branch is the one that is deployed
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

The application runs a client-side DB in the browser with the following configuration, so you don't need to run or configure a local database server at all.

```bash
# LobeChat Environment Configuration

# OpenAI Configuration
OPENAI_API_KEY="abc123"

# Anthropic Configuration
ANTHROPIC_API_KEY="abc123"

# Google Configuration
GOOGLE_API_KEY="abc123"

# Development Mode - Client-side database
NEXT_PUBLIC_SERVICE_MODE=
NEXT_PUBLIC_CLIENT_DB=pglite
NEXT_PUBLIC_IS_DESKTOP_APP=0
NEXT_PUBLIC_ENABLE_NEXT_AUTH=0

# Key Vaults Secret
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
APP_URL=http://localhost:3015

# Feature Flags
FEATURE_FLAGS=-check_updates,-market,-plugins,-openai_api_key,-welcome_suggest,-language_model_settings
```

### Installing dependencies

```bash
nix develop
pnpm install
```

### Running the application

```bash
nix develop
DATABASE_DRIVER=node pnpm dev
```
