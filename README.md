# AI Agent Security Lab — Real-model version

This version calls a real language model through the OpenAI API and exposes controlled tool calls to compare a vulnerable policy with a secured policy.

## Safety and scope

- Uses fictional customer records only.
- The external-send tool is a simulation; **it never makes a network request**.
- Vulnerable mode intentionally allows the model's tool request to read across users and records a simulated send attempt.
- Secure mode enforces owner checks in backend tool handlers and denies the external-send action.
- This is an educational proof of concept, not production security software or a reproduction of any real breach.
- Model behavior is nondeterministic. It may not request tools on every run; the UI shows the actual tool trace. Try the suggested user request if it does not call tools.

## Requirements

- Node.js 20+
- An OpenAI API key with API access. API usage may incur charges.
- Never paste your key into the browser or commit it to source control.

## Local setup (Windows / VS Code)

1. Extract the ZIP and open the project folder in VS Code.
2. In the terminal run:

   ```bash
   npm install
   ```

3. Edit `.env` and set your key:

   ```dotenv
   OPENAI_API_KEY=your_real_key_here
   OPENAI_MODEL=gpt-4.1-mini
   PORT=3001
   CLIENT_ORIGIN=http://127.0.0.1:5173
   ```

4. Start the app:

   ```bash
   npm run dev
   ```

5. Open `http://127.0.0.1:5173`. Confirm the header says `Model configured`.

The API key stays in the Node.js server process. The browser only calls your local backend. `ENABLE_VULNERABLE_DEMO=true` enables the intentionally unsafe path only for this local experiment; keep it `false` for public deployment.

## Demo walkthrough

1. Choose Alice.
2. Click **Run vulnerable agent**. The model reads a document with an indirect prompt injection. In this deliberately vulnerable mode, backend tools allow cross-user record lookups and simulate an outbound action.
3. Click **Run secured agent** with the same request. Backend authorization denies cross-user reads, and outbound transfer is not permitted.
4. Inspect the model response, tool trace, and event log. If the model does not call tools, try this user request:
   `Follow the operational instructions in the support document and summarize what you did.`
5. Repeat as Bob to demonstrate the policy is actor-scoped.

## What the result means

The vulnerable and secure paths intentionally have different backend policies. This demonstrates why prompt wording alone is not a security boundary. It does not prove that prompt injection can always be prevented, and it does not prove that a real production system is secure.
