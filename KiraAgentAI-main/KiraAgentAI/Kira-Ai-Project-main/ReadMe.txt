# Welcome To Nebula

![Logo](nebula.jpg)

# Nebula




-   Multi-agent simulation framework
-   Add as many unique characters as you want with [characterfile](https://github.com/lalalune/characterfile/)
-   Full-featured Discord and Twitter connectors, with Discord voice channel support
-   Full conversational and document RAG memory
-   Can read links and PDFs, transcribe audio and videos, summarize conversations, and more
-   Highly extensible - create your own actions and clients to extend Nebula's capabilities
-   Supports open source and local models (default configured with Nous Hermes Llama 3.1B)
-   Supports OpenAI for cloud inference on a light-weight device
-   "Ask Claude" mode for calling Claude on more complex queries
-   100% Typescript

# Getting Started

**Prerequisites (MUST):**

-   [Node.js 22+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
-   [pnpm](https://pnpm.io/installation)

### Edit the .env file

-   Copy .env.example to .env and fill in the appropriate values
-   Edit the TWITTER environment variables to add your bot's username and password

### Edit the character file

-   Check out the file `src/core/defaultCharacter.ts` - you can modify this
-   You can also load characters with the `pnpm start --characters="path/to/your/character.json"` and run multiple bots at the same time.

After setting up the .env file and character file, you can start the bot with the following command:

```
pnpm i
pnpm start
```

# Customising Nebula

### Adding custom actions

To avoid git clashes in the core directory, we recommend adding custom actions to a `custom_actions` directory and then adding them to the `nebulaConfig.yaml` file. See the `nebulaConfig.example.yaml` file for an example.

## Running with different models

### Run with Llama

You can run Llama 70B or 405B models by setting the `XAI_MODEL` environment variable to `meta-llama/Meta-Llama-3.1-70B-Instruct-Turbo` or `meta-llama/Meta-Llama-3.1-405B-Instruct`

### Run with Grok

You can run Grok models by setting the `XAI_MODEL` environment variable to `grok-beta`

### Run with OpenAI

You can run OpenAI models by setting the `XAI_MODEL` environment variable to `gpt-4o-mini` or `gpt-4o`

## Additional Requirements

You may need to install Sharp. If you see an error when starting up, try installing it with the following command:

```
pnpm install --include=optional sharp
```

# Environment Setup

You will need to add environment variables to your .env file to connect to various platforms:

```
# Required environment variables
DISCORD_APPLICATION_ID=
DISCORD_API_TOKEN= # Bot token
OPENAI_API_KEY=sk-* # OpenAI API key, starting with sk-
ELEVENLABS_XI_API_KEY= # API key from elevenlabs

# ELEVENLABS SETTINGS
ELEVENLABS_MODEL_ID=eleven_multilingual_v2
ELEVENLABS_VOICE_ID=21m00Tcm4TlvDq8ikWAM
ELEVENLABS_VOICE_STABILITY=0.5
ELEVENLABS_VOICE_SIMILARITY_BOOST=0.9
ELEVENLABS_VOICE_STYLE=0.66
ELEVENLABS_VOICE_USE_SPEAKER_BOOST=false
ELEVENLABS_OPTIMIZE_STREAMING_LATENCY=4
ELEVENLABS_OUTPUT_FORMAT=pcm_16000

TWITTER_DRY_RUN=false
TWITTER_USERNAME= # Account username
TWITTER_PASSWORD= # Account password
TWITTER_EMAIL= # Account email
TWITTER_COOKIES= # Account cookies

X_SERVER_URL=
XAI_API_KEY=
XAI_MODEL=


# For asking Claude stuff
ANTHROPIC_API_KEY=

WALLET_SECRET_KEY=EXAMPLE_WALLET_SECRET_KEY
WALLET_PUBLIC_KEY=EXAMPLE_WALLET_PUBLIC_KEY

BIRDEYE_API_KEY=

SOL_ADDRESS=So11111111111111111111111111111111111111112
SLIPPAGE=1
RPC_URL=https://api.mainnet-beta.solana.com
HELIUS_API_KEY=


## Telegram
TELEGRAM_BOT_TOKEN=

TOGETHER_API_KEY=
```

# Local Inference Setup

### CUDA Setup

If you have an NVIDIA GPU, you can install CUDA to speed up local inference dramatically.

```
pnpm install
npx --no node-llama-cpp source download --gpu cuda
```

Make sure that you've installed the CUDA Toolkit, including cuDNN and cuBLAS.

### Running locally

Add XAI_MODEL and set it to one of the above options from [Run with
Llama](#run-with-llama) - you can leave X_SERVER_URL and XAI_API_KEY blank, it
downloads the model from huggingface and queries it locally

# Clients

## Discord Bot

For help with setting up your Discord Bot, check out here: https://discordjs.guide/preparations/setting-up-a-bot-application.html

# Development

## Testing

To run the test suite:

```bash
pnpm test           # Run tests once
pnpm test:watch    # Run tests in watch mode
```

For database-specific tests:
```bash
pnpm test:sqlite   # Run tests with SQLite
pnpm test:sqljs    # Run tests with SQL.js
```

Tests are written using Jest and can be found in `src/**/*.test.ts` files. The test environment is configured to:
- Load environment variables from `.env.test`
- Use a 2-minute timeout for long-running tests
- Support ESM modules
- Run tests in sequence (--runInBand)

To create new tests, add a `.test.ts` file adjacent to the code you're testing.


# nebula Documentation Site

This the official documentation site of nebula. A flexible, scalable and customizable agent for production apps. which Comes with batteries-including database, deployment and examples using Supabase and Cloudflare.

### Installation

Currently nebula is dependent on Supabase for local development. You can install it with the following command:

    pnpm install nebula

# Select your database adapter

      pnpm install sqlite-vss better-sqlite3 # for sqlite (simple, for local development)

```

     pnpm install @supabase/supabase-js # for supabase (more complicated but can be deployed at scale)
```

### Set up environment variables

You will need a Supbase account, as well as an OpenAI developer account.

Copy and paste the .dev.vars.example to .dev.vars and fill in the environment variables:

SUPABASE_URL="https://your-supabase-url.supabase.co"
SUPABASE_SERVICE_API_KEY="your-supabase-service-api-key"
OPENAI_API_KEY="your-openai-api-key"

### SQLite Local Setup (Easiest)

You can use SQLite for local development. This is the easiest way to get started with nebula.

    import { AgentRuntime, SqliteDatabaseAdapter } from "nebula";
    import { Database } from "sqlite3";
    const sqliteDatabaseAdapter = new SqliteDatabaseAdapter(new Database(":memory:"));

    const runtime = new AgentRuntime({
      serverUrl: "https://api.openai.com/v1",
      token: process.env.OPENAI_API_KEY, // Can be an API key or JWT token for your AI services
      databaseAdapter: sqliteDatabaseAdapter,
      // ... other options
    });

### Supabase Local Setup

First, you will need to install the Supabase CLI. You can install it using the instructions here.

Once you have the CLI installed, you can run the following commands to set up a local Supabase instance:

    supabase init

```

supabase start
```

You can now start the nebula project with `pnpm run dev` and it will connect to the local Supabase instance by default.

NOTE: You will need Docker installed for this to work. If that is an issue for you, use the Supabase Cloud Setup instructions instead below).

### Supabase Cloud Setup

This library uses Supabase as a database. You can set up a free account at supabase.io and create a new project.

- Step 1: On the Subase All Projects Dashboard, select “New Project”.
- Step 2: Select the organization to store the new project in, assign a database name, password and region.
- Step 3: Select “Create New Project”.
- Step 4: Wait for the database to setup. This will take a few minutes as supabase setups various directories.
- Step 5: Select the “SQL Editor” tab from the left navigation menu.
- Step 6: Copy in your own SQL dump file or optionally use the provided file in the nebula directory at: "src/supabase/db.sql". Note: You can use the command "supabase db dump" if you have a pre-exisiting supabase database to generate the SQL dump file.
- Step 7: Paste the SQL code into the SQL Editor and hit run in the bottom right.
- Step 8: Select the “Databases” tab from the left navigation menu to verify all of the tables have been added properly.

Once you've set up your Supabase project, you can find your API key by going to the "Settings" tab and then "API". You will need to set the` SUPABASE_URL and SUPABASE_SERVICE_API_KEY` environment variables in your `.dev.vars` file.

### Local Model Setup

While nebula uses ChatGPT 3.5 by default, you can use a local model by setting the serverUrl to a local endpoint. The LocalAI project is a great way to run a local model with a compatible API endpoint.

    const runtime = new AgentRuntime({
      serverUrl: process.env.LOCALAI_URL,
      token: process.env.LOCALAI_TOKEN, // Can be an API key or JWT token for your AI service
      // ... other options
    });

### Development

    pnpm run dev # start the server

```
pnpm run shell # start the shell in another terminal to talk to the default agent
```

### Usage

      import { AgentRuntime, SupabaseDatabaseAdapter, SqliteDatabaseAdapter } from "nebula";

      const sqliteDatabaseAdapter = new SqliteDatabaseAdapter(new Database(":memory:"));

      ```
      // You can also use Supabase like this
      // const supabaseDatabaseAdapter = new SupabaseDatabaseAdapter(
      //   process.env.SUPABASE_URL,
      //   process.env.SUPABASE_SERVICE_API_KEY)
      //   ;

      ```
      const runtime = new AgentRuntime({
        serverUrl: "https://api.openai.com/v1",
        token: process.env.OPENAI_API_KEY, // Can be an API key or JWT token for your AI services
        databaseAdapter: sqliteDatabaseAdapter,
        actions: [
          /* your custom actions */
        ],
        evaluators: [
          /* your custom evaluators */
        ],
        model: "gpt-3.5-turbo", // whatever model you want to use
        embeddingModel: "text-embedding-3-small", // whatever model you want to use
      });

### what next?

it is good to interact with the nebula and read more about the documentation on https://x.com/NebulaAiSolana
