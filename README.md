# USDT Transaction Explorer

USDT Transaction Explorer is a web application for exploring wallet transaction relationships and receiving a concise AI-generated interpretation of the resulting transaction tree. It accepts Ethereum-style and TRON-style wallet addresses, retrieves recent transactions from public explorer APIs, and presents the relationships as an interactive D3.js tree.

The project was developed as a B.Tech minor project at IILM University, Greater Noida.

> **Important:** The AI output is informational only. It is not a financial, legal, compliance, or fraud determination, and it should not be the only basis for deciding whether to transact with a wallet.

## What it does

- Detects the target network from the wallet-address prefix: `T` for TRON and other addresses for Ethereum-style lookups.
- Retrieves recent transaction records through the Etherscan and Tronscan APIs.
- Recursively builds a wallet relationship tree with circular-path avoidance.
- Runs processing in a background thread and exposes progress updates to the browser.
- Draws a zoomable, pannable, draggable transaction tree with D3.js.
- Sends the completed tree to Google Gemini for a short, plain-language wallet assessment.
- Includes stub response files for local demonstrations when live APIs are not enabled.

## Project structure

```text
.
├── app.py                         # Flask server, explorer API integration, tree building
├── ai.py                          # Google Gemini analysis integration
├── templates/
│   └── graph.html                 # D3.js web interface and visualization
├── response.json                  # Stub transaction response for local testing
├── response2.json                 # Additional sample transaction-tree data
└── README.md
```

## Technology stack

| Area | Technology |
| --- | --- |
| Backend | Python, Flask |
| HTTP client | Requests |
| Visualization | D3.js v7 |
| Blockchain data | Etherscan API, Tronscan API |
| AI analysis | Google Generative AI, Gemini 1.5 Flash |
| Client | HTML, CSS, JavaScript |

## Getting started

### Prerequisites

- Python 3.10 or later
- An Etherscan API key
- A Google Generative AI API key
- Internet access for blockchain APIs, Gemini, and the D3.js CDN

### Installation

1. Clone the repository and enter the project folder.

   ```bash
   git clone <your-repository-url>
   cd usdt-transaction-explorer
   ```

2. Create and activate a virtual environment.

   ```bash
   python -m venv .venv
   # Windows PowerShell
   .\.venv\Scripts\Activate.ps1
   # macOS/Linux
   source .venv/bin/activate
   ```

3. Install the dependencies.

   ```bash
   pip install flask requests google-generativeai
   ```

4. Add your credentials.

   - In `app.py`, replace the Etherscan key placeholder with your key.
   - In `ai.py`, replace the Google Generative AI key placeholder with your key.
   - The current source also contains a Tronscan key placeholder, although the included request does not send that value.

   Never commit real API keys. For a production-ready version, move credentials to environment variables or a local `.env` file excluded by `.gitignore`.

5. Start the application.

   ```bash
   python app.py
   ```

6. Open `http://127.0.0.1:5000` in a browser, enter a wallet address, and submit it for analysis.

## Local stub mode

For a UI demonstration without live explorer calls, set the following value in `app.py`:

```python
USE_STUB = True
```

The application will then read `response.json`. Stub mode is useful for checking the visualization flow, but it does not represent a live wallet analysis.

## Application flow

```text
Wallet address
    ↓
Network detection
    ↓
Etherscan or Tronscan lookup
    ↓
Recursive transaction-tree builder
    ↓
Progress endpoint and result endpoint
    ↓
D3.js interactive visualization + Gemini wallet commentary
```

## API endpoints

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/` | `GET` | Serves the transaction explorer page |
| `/transaction-tree/erc20?address=<wallet>` | `GET` | Starts background transaction-tree generation and returns a request ID |
| `/transaction-tree/progress?request_id=<id>` | `GET` | Returns processing progress and wallet count |
| `/transaction-tree/result?request_id=<id>` | `GET` | Returns the completed transaction tree |
| `/transaction-tree/get-ai-analysis?request_id=<id>` | `GET` | Returns Gemini's analysis of the completed tree |

## Included project documentation

The repository can retain the accompanying academic deliverables alongside the source code. Their descriptions below distinguish documented goals from the current implementation.

| File | Description |
| --- | --- |
| `Info Sec Minor Project Report.pdf` | Software requirements specification describing scope, interfaces, functional requirements, and intended non-functional targets |
| `Minor Project Report.docx` | Full minor-project report covering motivation, design, implementation, limitations, and future scope |
| `USDT_Report.pdf` | PDF version of the project report |
| `USDT-Transaction-Explorer-Software-Requirements-Specification.pptx` | Presentation version of the software requirements specification |
| `usdt-transaction-explorer-master.zip` | Source-code archive for this application |

## Current limitations

- The included implementation relies on third-party explorer APIs and their rate limits.
- It currently limits each lookup to the first two filtered recent transactions per explored wallet, despite the larger API request limit in the TRON request.
- Network selection uses the address prefix and does not perform full address validation.
- Tree construction can become expensive at higher traversal depths or for highly connected wallets.
- The current AI analysis uses only the collected tree data and cannot establish that a wallet or transaction is legitimate.
- The supplied source embeds credential placeholders. Use environment-based configuration before deploying or sharing the project.
- The reports describe AWS/LinodeGPU deployment, autoscaling, CI/CD, and uptime targets; these are design goals and are not configured in the supplied source archive.

## Suggested improvements

- Add a `requirements.txt`, `.env.example`, and `.gitignore`.
- Store API keys in environment variables and validate configuration at startup.
- Validate addresses and explicitly filter transactions to the intended USDT token contracts.
- Add tests for API handlers, tree construction, and error conditions.
- Replace in-memory request state with a persistent job store for multi-user deployment.
- Add pagination, configurable traversal depth, export options, and additional supported chains.
- Treat AI output as an assistive signal alongside verifiable compliance and risk controls.

## Academic project team

- Riha Singh
- Shivangi Singh
- Prabhakar Joshi
- Amanul Haque

**Supervisor:** Dr. Basab Nath  
**Institution:** School of Computer Science and Engineering, IILM University, Greater Noida

## License

No license is included in the supplied project materials. Add a license before publishing or distributing the repository.
