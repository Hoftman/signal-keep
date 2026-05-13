# AI Development Tooling — State of the Market, May 2026

## The shape of the landscape

The "AI coding" space is not one market. It's at least five, and most public discussion conflates them. Cleanly separated:

1. **AI-assisted IDEs** — you write code, AI helps. Cursor, GitHub Copilot, Windsurf, Zed, Google Antigravity.
2. **Terminal-native coding agents** — agent runs in your shell, edits files, runs commands. Claude Code, Aider, Gemini CLI.
3. **Cloud IDEs** — full dev environment in a browser tab. Replit, GitHub Codespaces, Gitpod, CodeSandbox, StackBlitz, Firebase Studio.
4. **AI app builders ("vibe coding")** — describe an app, get a deployed app. Lovable, Bolt.new, v0, Mocha, Base44, Tempo, Magic Patterns, Dyad, Create.xyz, a0.dev, Rork, Databutton, HeyBoss, Co.dev.
5. **Autonomous coding agents** — hand off a goal, the agent works asynchronously. Devin, Jules, Factory, Cosine Genie.

Plus orthogonal layers: **LLM app frameworks** (LangChain/LangGraph, LlamaIndex, Vercel AI SDK) are libraries you import; **no-code platforms** (Bubble, Retool, Glide, Adalo, FlutterFlow) predate the AI wave and have bolted AI on.

## Who's winning, by axis

- **Install base / volume:** GitHub Copilot. ~4.7M paid subs, generating ~46% of code in repos where it's installed. Wins on distribution because it's a plugin, not an editor.
- **Revenue:** Cursor. ~$2B ARR, 1M+ paying users — the commercial leader of AI-native IDEs.
- **Developer satisfaction:** Claude Code. 46% most-loved in the JetBrains April 2026 survey vs. Cursor 19%, Copilot 9%.
- **App builders by valuation:** Lovable ($6.6B, late 2025) and Replit ($10M → $100M ARR in nine months) lead the vibe-coding category.

The dominant working pattern among professionals: **2–4 tools stacked**, typically Cursor for editing + Claude Code for heavier agentic work. Single-tool loyalty is rare.

## Trends actually worth tracking

- **Agent-first IDEs.** Antigravity (Nov 2025) bet the whole product on agents-as-first-class-citizens with their own workspace, artifacts, and parallel execution. Cursor, Windsurf, and Copilot are racing to match. The chat-in-sidebar paradigm is dying.
- **Pricing shifting to usage-based.** Cursor switched in 2025; Copilot moves to AI Credits June 1, 2026. Flat-rate AI tooling is going away.
- **Browser-native runtimes maturing.** WebContainers (StackBlitz/Bolt) and Firebase Studio show that "real Node.js in the browser tab" is viable for prototyping, though still resource-heavy.
- **The technical cliff in app builders.** Tools demo beautifully but stall when users need real auth, RLS policies, custom backend logic. Most "vibe-coded" apps don't survive contact with production.
- **Mobile is the next front.** a0.dev, Rork, FlutterFlow, Draftbit pushing native mobile generation — still rougher than web.

## Real gaps

- **Production handoff.** No tool cleanly bridges "AI built it" to "humans maintain it for five years." Code quality varies wildly; debugging AI-generated systems is its own skill nobody has yet.
- **Trust and verification.** Stack Overflow 2025: 84% adoption, only 29% trust. Agents work, developers don't believe them. Artifacts, screenshots, and walkthroughs (Antigravity's bet) are early attempts to close this.
- **Enterprise compliance.** Replit has SOC 2 Type II; most app builders don't. Regulated industries are still mostly locked out of vibe coding.
- **Mobile-native and offline-first.** Still poorly served across the board.
- **The non-developer ceiling.** Lovable and Mocha get further than ever, but the moment something breaks at the database or auth layer, non-technical users are stranded.

## The honest read

The boundary between categories is dissolving. Replit added a real agent; Cursor added a manager surface; Antigravity ships with browser orchestration; Lovable generates code clean enough that developers will accept it. Within 12–18 months "IDE vs. app builder vs. agent platform" will probably stop being a useful distinction — they're all converging on **agent-orchestrated development with human review surfaces**.

The winners won't be the ones with the cleverest models. They'll be the ones who solve trust, handoff, and the long tail of "it almost works."
