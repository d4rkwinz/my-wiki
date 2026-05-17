---
tags: [stack, free-tier, side-projects, saas, reference]
sources:
  - "Cheapest Niches For Programming 2026 — curated free-tier list (2026-05-17)"
status: published
updated: 2026-05-17
---

## 💸 Cheap Side-Project Stack 2026

Reference taxonomy of free or near-free services for shipping a side project / SaaS on minimum budget. Each section lists viable options with their free-tier limit in parentheses.

> **How to use:** pick one service per category. Most side projects ship on **Cloudflare Pages + Workers + D1 + R2** with auth via Clerk/WorkOS and email via Resend — that whole stack stays $0/mo until you have real traffic.

---

### 🌐 Frontend Hosting

| Service              | Free tier                      | Notes                         |
| -------------------- | ------------------------------ | ----------------------------- |
| **Cloudflare Pages** | Unlimited bandwidth            | Best free tier; pairs with Workers |
| **Vercel**           | 100 GB hobby                   | Best DX for Next.js           |
| **Netlify**          | 100 GB                         | Solid generalist              |
| **GitHub Pages**     | Static only                    | Free for OSS / docs sites     |
| **Render**           | Free static                    | Free tier sleeps              |

### ⚙️ Backend / Serverless

| Service                | Free tier                | Notes                                |
| ---------------------- | ------------------------ | ------------------------------------ |
| **Cloudflare Workers** | 100k requests/day        | Edge runtime, no cold start          |
| **Render**             | Free instance, sleeps 15m | OK for low-traffic APIs             |
| **Deno Deploy**        | Free tier                | Native TypeScript                    |
| **Val.town**           | Free runs                | Tiny scripts / glue / cron           |
| **Koyeb**              | 1 free instance          | Containerized apps                   |

### 🗄️ Databases

| Service              | Free tier                          | Notes                              |
| -------------------- | ---------------------------------- | ---------------------------------- |
| **Neon Postgres**    | Scale-to-zero, generous            | Postgres with branching            |
| **Supabase**         | 500 MB, pauses after 7d            | Postgres + Auth + Storage bundle   |
| **Turso**            | 5 GB, 500M reads                   | SQLite at the edge (libSQL)        |
| **Cloudflare D1**    | 5 GB                               | SQLite, integrates with Workers    |
| **MongoDB Atlas**    | 512 MB                             | If you need a document store       |
| **Upstash Redis**    | 10k commands/day                   | Serverless Redis                   |

### 📦 Storage / CDN

| Service             | Free tier                  | Notes                                 |
| ------------------- | -------------------------- | ------------------------------------- |
| **Cloudflare R2**   | 10 GB, **no egress fees**  | S3-compatible; egress is the killer elsewhere |
| **Backblaze B2**    | 10 GB                      | Cheapest paid tier beyond free        |
| **Bunny.net**       | Pay-as-you-go              | Cheapest CDN overall                  |
| **ImageKit**        | 20 GB bandwidth            | Image CDN + transformations           |
| **UploadThing**     | 2 GB                       | Drop-in file uploads for apps         |

### 🔐 Auth Services

| Service              | Free tier              | Notes                                  |
| -------------------- | ---------------------- | -------------------------------------- |
| **WorkOS AuthKit**   | 1M MAU                 | Best free ceiling; enterprise-ready    |
| **Clerk**            | 50k MAU                | Best DX, prebuilt UI components        |
| **Supabase Auth**    | 50k MAU                | Bundled if you're already on Supabase  |
| **Better-Auth**      | OSS, self-host         | Modern TS auth library                 |
| **Auth.js (NextAuth)** | OSS, self-host       | Battle-tested                          |
| **Kinde**            | 10.5k MAU              | Indie-friendly billing                 |

### ✉️ Email / SMS

| Service        | Free tier              | Notes                              |
| -------------- | ---------------------- | ---------------------------------- |
| **Resend**     | 3k/mo, 100/day         | Best DX; React Email integration   |
| **Brevo**      | 300/day                | Marketing + transactional combo    |
| **AWS SES**    | Pay-as-you-go, dirt cheap | At scale this wins on price     |
| **Loops**      | 1k contacts            | Lifecycle / drip campaigns         |
| **Mailtrap**   | Sandbox free           | For dev/staging email capture      |

### 📊 Monitoring / Analytics

| Service              | Free tier                       | Notes                                  |
| -------------------- | ------------------------------- | -------------------------------------- |
| **PostHog**          | 1M events, 100k errors          | Analytics + session replay + feature flags + LLM observability |
| **BetterStack**      | 100k exceptions                 | Logs + uptime + status pages bundle    |
| **Sentry**           | 5k errors                       | Industry default for error tracking    |
| **Axiom**            | 500 GB logs                     | Cheap log storage                      |
| **Plausible / Umami** | OSS, self-host                 | Privacy-first analytics                |
| **UptimeRobot**      | 50 monitors                     | Free uptime checks                     |

### 🔎 Search / Vectors / Jobs

| Service                       | Free tier        | Notes                                |
| ----------------------------- | ---------------- | ------------------------------------ |
| **Meilisearch / Typesense**   | OSS, self-host   | Algolia alternatives                 |
| **Algolia**                   | 10k records      | Hosted full-text search              |
| **Qdrant Cloud**              | 1 GB             | Vector DB for RAG                    |
| **Pinecone**                  | Starter tier     | Managed vector DB                    |
| **Inngest / Trigger.dev**     | Free tier        | Background jobs / workflows / cron   |

### 🤖 AI APIs (free tiers)

| Service                  | Free tier                       | Notes                                |
| ------------------------ | ------------------------------- | ------------------------------------ |
| **Google AI Studio**     | 1.5k Gemini requests/day        | Best free volume for prototyping     |
| **Groq**                 | Llama 3.3 70B, very fast        | Inference speed leader               |
| **Cerebras**             | 1M tokens/day                   | Generous token budget                |
| **OpenRouter**           | Free models available           | One API for 100+ models              |
| **Mistral**              | 1B tokens/mo                    | Free EU-hosted models                |
| **GitHub Models**        | Free GPT access                 | For prototyping with OpenAI-class    |

### 🔁 CI/CD

| Service                | Free tier              | Notes                                  |
| ---------------------- | ---------------------- | -------------------------------------- |
| **GitHub Actions**     | 2k minutes/mo          | Default for OSS / GitHub-hosted repos  |
| **CircleCI**           | 6k minutes/mo          | Best free CI minutes                   |
| **GitLab CI**          | 400 minutes            | Bundled with GitLab repos              |
| **Google Cloud Build** | 120 min/day            | If you're already on GCP               |
| **Self-host Jenkins / Drone** | Free OSS         | When CI minutes become the bottleneck  |

### 💳 Payments

| Service              | Fees                       | Notes                                       |
| -------------------- | -------------------------- | ------------------------------------------- |
| **Stripe**           | 2.9% + 30¢, no monthly     | Default for SaaS, but you handle tax        |
| **Polar**            | Lowest SaaS fees, MoR      | Indie-friendly                              |
| **Lemon Squeezy**    | MoR — they handle tax/VAT  | Easiest path to global SaaS                 |
| **Paddle**           | MoR alternative            | Established MoR, decent fees                |
| **PayPal**           | Standard fees              | Use for consumer reach, not SaaS primary    |

> **MoR** = Merchant of Record — they handle global tax / VAT compliance for you. Worth the slightly higher fees if you're solo.

### 🛠️ Dev Tools / IDEs

| Service                     | Free tier                 | Notes                                    |
| --------------------------- | ------------------------- | ---------------------------------------- |
| **GitHub**                  | Free private repos        | Default                                  |
| **VSCode / Zed**            | Free                      | Zed for speed; VSCode for ecosystem      |
| **Cursor / Claude Code**    | Free tier                 | AI coding agents                         |
| **Warp Terminal**           | Free                      | AI-native terminal                       |
| **Bruno**                   | OSS                       | Postman alternative, git-friendly        |

---

## 🎯 Reference combos

A few sane $0/mo stacks pulled from the above:

| Use case            | Stack                                                                                  |
| ------------------- | -------------------------------------------------------------------------------------- |
| **Static landing**  | GitHub Pages + Plausible                                                               |
| **Next.js SaaS**    | Vercel + Neon + Clerk + Resend + Sentry + Stripe                                       |
| **Edge-native SaaS** | Cloudflare Pages + Workers + D1 + R2 + WorkOS + Resend + PostHog + Polar              |
| **AI side project** | Cloudflare Workers + Groq/Gemini + Qdrant + Upstash Redis + Vercel                     |
| **OSS-first / self-host** | Coolify VPS + Meilisearch + Plausible + Better-Auth + Stripe                     |

---

## 🔖 Related

- [[notes/repos/index|repos]] — for OSS implementations and tooling around this stack
- [[Top 1% Claude Code Playbook]] — for the AI coding agent layer above
