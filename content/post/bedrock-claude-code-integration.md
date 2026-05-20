+++
author = "Yaheya Quazi"
title = "Agentic Coding Meets Enterprise Security"
date = "2026-05-20"
description = "Agentic Coding Meets Enterprise Security:Deep Diving into Claude Code on Amazon Bedrock"
tags = [
"technology"
]
+++

# Agentic Coding Meets Enterprise Security: Deep Diving into Claude Code on Amazon Bedrock

AI-powered development has evolved far beyond basic autocomplete. Tools like **Claude Code**—Anthropic’s agentic command-line assistant—can actively read codebases, run tests, repair bugs, and manage Git repositories through simple natural language workflows. 

However, deploying an autonomous coding agent across an entire engineering organization requires navigating stringent security, compliance, and identity guardrails. You can't just hand out individual API keys to hundreds of developers. 

This is where **Amazon Bedrock** enters the picture. By routing Claude Code through Amazon Bedrock, enterprises can leverage cutting-edge agentic capabilities while retaining absolute control over data boundaries, access governance, and infrastructure costs. 

Let's break down how this integration works, why it matters, and how to harness advanced new capabilities like the **Mantle endpoint**.

---

### Why Run Claude Code on Amazon Bedrock?

Integrating Claude Code with Amazon Bedrock bridges the gap between raw developer velocity and robust enterprise security:

* **Data Perimeter & Compliance:** Your code snippets and prompts stay securely within your AWS environment, ensuring data residency requirements (such as EU-only data routing) are strictly maintained.
* **Unified Identity Governance:** Say goodbye to fragmented API key management. Developers authenticate using your existing AWS IAM or IAM Identity Center (SSO). If an employee leaves the company, revoking their AWS access automatically revokes their Claude Code access.
* **Consolidated Billing:** All token consumption maps cleanly to your AWS bill. You can track, tag, and budget your AI spend side-by-side with your standard EC2 and S3 resources.
* **Enterprise Guardrails:** You can natively overlay Amazon Bedrock Guardrails to block sensitive data, redact personally identifiable information (PII), and enforce architectural or topical restrictions at the infrastructure layer.

---

### Setting Up: From Frictionless Wizard to Manual Scale

Anthropic provides two main paths to wire up Claude Code to Bedrock: an interactive terminal setup wizard or manual environment configuration.

#### Path A: The Guided Login Wizard
For local developers with active AWS credentials, setup is entirely hands-off. Simply run:

```

```text
File saved successfully as claude_code_bedrock_blog.md

```bash
claude

```

At the authentication prompt, select **3rd-party platform**, then choose **Amazon Bedrock**. The interactive wizard automatically scans your `~/.aws` directory, detects active profiles, checks your region, and validates which Claude models your account can invoke. It then securely appends these parameters to your user settings file (`~/.claude/settings.json`).

#### Path B: Manual Configuration (Ideal for CI/CD or Scripted Rollouts)

If you are automating developer environments or configuring remote containers, you can skip the wizard entirely by setting explicit environment variables:

```bash
# Enable the Bedrock integration
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1

# Authenticate via your preferred AWS credential method
aws sso login --profile=engineering-profile
export AWS_PROFILE=engineering-profile

```

---

### Fine-Tuning the Engine: Model Pinning & The 1M Context Window

By default, Claude Code uses flexible aliases that automatically resolve to the latest versions of frontier models like Claude Sonnet. While this is great for individual users, enterprises usually prefer deterministic environments.

#### Pinning Versions

To prevent unexpected shifts when a model updates, you can pin explicit model IDs or cross-region inference profiles:

```bash
export ANTHROPIC_DEFAULT_SONNET_MODEL='us.anthropic.claude-sonnet-4-6'
export ANTHROPIC_DEFAULT_OPUS_MODEL='us.anthropic.claude-opus-4-7'
export ANTHROPIC_DEFAULT_HAIKU_MODEL='us.anthropic.claude-haiku-4-5-20251001-v1:0'

```

#### Unlocking the 1M Token Context Window

When tackling massive code refactors or analyzing sprawling legacy monorepos, context is king. Claude Opus and Sonnet models support a staggering **1 million token context window** on Amazon Bedrock.

Claude Code automatically opens up this massive reasoning space when you select a 1M model variant. If you are configuring your environment manually, you can explicitly request it by appending `[1m]` to your pinned model ID strings.

---

### The Secret Sauce: Meet the "Mantle" Endpoint

One of the most powerful features buried in the Claude Code architecture is support for the **Mantle endpoint**.

Conventionally, using Bedrock means translating requests into the AWS Bedrock Invoke API payload structure. **Mantle** flips this paradigm. It is an Amazon Bedrock endpoint that serves Claude models using the *native Anthropic API payload shape*, while completely preserving your AWS credentials, IAM permissions, and SSO configurations.

#### Why use Mantle?

It allows your organization to consume native-format model configurations seamlessly over AWS infrastructure. Furthermore, Claude Code allows you to run the Bedrock Invoke API and the Mantle endpoint **side-by-side** in the same terminal session.

To activate Mantle alongside standard Bedrock routing, export both flags:

```bash
export CLAUDE_CODE_USE_BEDROCK=1
export CLAUDE_CODE_USE_MANTLE=1
export AWS_REGION=us-east-1

```

When this hybrid mode is enabled, standard inference profiles route cleanly through the Bedrock Invoke API, while model IDs prefixed with the native format (e.g., `anthropic.claude-haiku-4-5`) automatically detour through Mantle. Typing `/status` inside your Claude Code interface will proudly display: `Amazon Bedrock + Amazon Bedrock (Mantle)`.

#### Routing Through Centralized LLM Gateways

If your enterprise routes all AI traffic through a centralized internal LLM gateway that securely injects AWS signatures on the server side, you can instruct Claude Code to bypass client-side SigV4 signing:

```bash
export CLAUDE_CODE_USE_MANTLE=1
export CLAUDE_CODE_SKIP_MANTLE_AUTH=1
export ANTHROPIC_BEDROCK_MANTLE_BASE_URL=[https://your-internal-gateway.enterprise.com](https://your-internal-gateway.enterprise.com)

```

---

### Pro-Tips & Troubleshooting

* **SSO Re-Auth Loops:** If your corporate proxy or VPN interrupts background authentication checks, it can trigger an infinite browser tab spawning loop via the automatic `awsAuthRefresh` feature. If this happens, simply strip the `awsAuthRefresh` property from your `~/.claude/settings.json` and rely on running `aws sso login` manually before your coding sessions.
* **On-Demand Throughput Errors:** If Bedrock throws an error stating that on-demand throughput isn't supported for your model, ensure you are specifying the model as an *inference profile ID* (e.g., using the `us.` or `eu.` region prefixes), which optimizes routing and ensures proper on-demand availability.
* **IAM Minimum Viable Policy:** To guarantee smooth operations, make sure your developer IAM roles include `bedrock:InvokeModelWithResponseStream` as well as `bedrock:GetInferenceProfile`—the latter prevents an extra network round-trip by allowing Claude Code to check model payload shapes before firing requests.

### Conclusion

By deploying Claude Code through Amazon Bedrock, organizations don't have to compromise. Developers get the fluid, autonomous command-line experience of a world-class AI agent that can rewrite code and fix bugs locally, while security teams rest easy knowing that data boundaries, IAM controls, and cost governance remain entirely under lock and key within AWS.

*Ready to streamline your enterprise development workflows? Head over to the official Claude Code Documentation Hub to explore advanced configuration files and multi-account scaling strategies.*
"""

filename = "claude_code_bedrock_blog.md"
with open(filename, "w", encoding="utf-8") as f:
f.write(md_content)

print(f"File saved successfully as {filename}")

```
Your MD file is ready
[file-tag: code-generated-file-0-1779288521280980626]

Here is the raw Markdown content for you to copy and paste directly:

```markdown
# Agentic Coding Meets Enterprise Security: Deep Diving into Claude Code on Amazon Bedrock

AI-powered development has evolved far beyond basic autocomplete. Tools like **Claude Code**—Anthropic’s agentic command-line assistant—can actively read codebases, run tests, repair bugs, and manage Git repositories through simple natural language workflows. 

However, deploying an autonomous coding agent across an entire engineering organization requires navigating stringent security, compliance, and identity guardrails. You can't just hand out individual API keys to hundreds of developers. 

This is where **Amazon Bedrock** enters the picture. By routing Claude Code through Amazon Bedrock, enterprises can leverage cutting-edge agentic capabilities while retaining absolute control over data boundaries, access governance, and infrastructure costs. 

Let's break down how this integration works, why it matters, and how to harness advanced new capabilities like the **Mantle endpoint**.

---

### Why Run Claude Code on Amazon Bedrock?

Integrating Claude Code with Amazon Bedrock bridges the gap between raw developer velocity and robust enterprise security:

* **Data Perimeter & Compliance:** Your code snippets and prompts stay securely within your AWS environment, ensuring data residency requirements (such as EU-only data routing) are strictly maintained.
* **Unified Identity Governance:** Say goodbye to fragmented API key management. Developers authenticate using your existing AWS IAM or IAM Identity Center (SSO). If an employee leaves the company, revoking their AWS access automatically revokes their Claude Code access.
* **Consolidated Billing:** All token consumption maps cleanly to your AWS bill. You can track, tag, and budget your AI spend side-by-side with your standard EC2 and S3 resources.
* **Enterprise Guardrails:** You can natively overlay Amazon Bedrock Guardrails to block sensitive data, redact personally identifiable information (PII), and enforce architectural or topical restrictions at the infrastructure layer.

---

### Setting Up: From Frictionless Wizard to Manual Scale

Anthropic provides two main paths to wire up Claude Code to Bedrock: an interactive terminal setup wizard or manual environment configuration.

#### Path A: The Guided Login Wizard
For local developers with active AWS credentials, setup is entirely hands-off. Simply run:
```bash
claude

```

At the authentication prompt, select **3rd-party platform**, then choose **Amazon Bedrock**. The interactive wizard automatically scans your `~/.aws` directory, detects active profiles, checks your region, and validates which Claude models your account can invoke. It then securely appends these parameters to your user settings file (`~/.claude/settings.json`).

#### Path B: Manual Configuration (Ideal for CI/CD or Scripted Rollouts)

If you are automating developer environments or configuring remote containers, you can skip the wizard entirely by setting explicit environment variables:

```bash
# Enable the Bedrock integration
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1

# Authenticate via your preferred AWS credential method
aws sso login --profile=engineering-profile
export AWS_PROFILE=engineering-profile

```

---

### Fine-Tuning the Engine: Model Pinning & The 1M Context Window

By default, Claude Code uses flexible aliases that automatically resolve to the latest versions of frontier models like Claude Sonnet. While this is great for individual users, enterprises usually prefer deterministic environments.

#### Pinning Versions

To prevent unexpected shifts when a model updates, you can pin explicit model IDs or cross-region inference profiles:

```bash
export ANTHROPIC_DEFAULT_SONNET_MODEL='us.anthropic.claude-sonnet-4-6'
export ANTHROPIC_DEFAULT_OPUS_MODEL='us.anthropic.claude-opus-4-7'
export ANTHROPIC_DEFAULT_HAIKU_MODEL='us.anthropic.claude-haiku-4-5-20251001-v1:0'

```

#### Unlocking the 1M Token Context Window

When tackling massive code refactors or analyzing sprawling legacy monorepos, context is king. Claude Opus and Sonnet models support a staggering **1 million token context window** on Amazon Bedrock.

Claude Code automatically opens up this massive reasoning space when you select a 1M model variant. If you are configuring your environment manually, you can explicitly request it by appending `[1m]` to your pinned model ID strings.

---

### The Secret Sauce: Meet the "Mantle" Endpoint

One of the most powerful features buried in the Claude Code architecture is support for the **Mantle endpoint**.

Conventionally, using Bedrock means translating requests into the AWS Bedrock Invoke API payload structure. **Mantle** flips this paradigm. It is an Amazon Bedrock endpoint that serves Claude models using the *native Anthropic API payload shape*, while completely preserving your AWS credentials, IAM permissions, and SSO configurations.

#### Why use Mantle?

It allows your organization to consume native-format model configurations seamlessly over AWS infrastructure. Furthermore, Claude Code allows you to run the Bedrock Invoke API and the Mantle endpoint **side-by-side** in the same terminal session.

To activate Mantle alongside standard Bedrock routing, export both flags:

```bash
export CLAUDE_CODE_USE_BEDROCK=1
export CLAUDE_CODE_USE_MANTLE=1
export AWS_REGION=us-east-1

```

When this hybrid mode is enabled, standard inference profiles route cleanly through the Bedrock Invoke API, while model IDs prefixed with the native format (e.g., `anthropic.claude-haiku-4-5`) automatically detour through Mantle. Typing `/status` inside your Claude Code interface will proudly display: `Amazon Bedrock + Amazon Bedrock (Mantle)`.

#### Routing Through Centralized LLM Gateways

If your enterprise routes all AI traffic through a centralized internal LLM gateway that securely injects AWS signatures on the server side, you can instruct Claude Code to bypass client-side SigV4 signing:

```bash
export CLAUDE_CODE_USE_MANTLE=1
export CLAUDE_CODE_SKIP_MANTLE_AUTH=1
export ANTHROPIC_BEDROCK_MANTLE_BASE_URL=[https://your-internal-gateway.enterprise.com](https://your-internal-gateway.enterprise.com)

```

---

### Pro-Tips & Troubleshooting

* **SSO Re-Auth Loops:** If your corporate proxy or VPN interrupts background authentication checks, it can trigger an infinite browser tab spawning loop via the automatic `awsAuthRefresh` feature. If this happens, simply strip the `awsAuthRefresh` property from your `~/.claude/settings.json` and rely on running `aws sso login` manually before your coding sessions.
* **On-Demand Throughput Errors:** If Bedrock throws an error stating that on-demand throughput isn't supported for your model, ensure you are specifying the model as an *inference profile ID* (e.g., using the `us.` or `eu.` region prefixes), which optimizes routing and ensures proper on-demand availability.
* **IAM Minimum Viable Policy:** To guarantee smooth operations, make sure your developer IAM roles include `bedrock:InvokeModelWithResponseStream` as well as `bedrock:GetInferenceProfile`—the latter prevents an extra network round-trip by allowing Claude Code to check model payload shapes before firing requests.

### Conclusion

By deploying Claude Code through Amazon Bedrock, organizations don't have to compromise. Developers get the fluid, autonomous command-line experience of a world-class AI agent that can rewrite code and fix bugs locally, while security teams rest easy knowing that data boundaries, IAM controls, and cost governance remain entirely under lock and key within AWS.

*Ready to streamline your enterprise development workflows? Head over to the official Claude Code Documentation Hub to explore advanced configuration files and multi-account scaling strategies.*

