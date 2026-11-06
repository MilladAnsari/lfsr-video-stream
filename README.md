# LFSR Encrypted Video Lab

This classroom project demonstrates the mechanics—and limitations—of a synchronous stream cipher. A Flask server reads one of the bundled videos, XORs each requested range with bytes from a 16-bit LFSR, and sends only ciphertext. A browser service worker decrypts ranges on demand, so playback can begin while later parts remain encrypted and unfetched.

## Run

Python 3.11 or newer is required.

```bash
python -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m stream_server --port 8000
```

Open <http://127.0.0.1:8000>, select a video, and press play when the first range is ready. Later ranges are decrypted during playback and seeking.

## Windows / PowerShell

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m stream_server --port 8000
```

## Verification and evaluation

Node.js is required for the JavaScript/browser contract tests.

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
.\.venv\Scripts\python.exe -m tools.evaluate
```

The evaluation runs the complete NistRng battery on equally sized LFSR, NLFSR,
and Geffe outputs. The full battery can take several minutes. Statistical
results do not establish cryptographic security.

Each statistical test receives its own copy of the bit sequence because
NistRng's matrix-rank test modifies its input. A regression test checks that
running matrix rank cannot change a later monobit result. NistRng selects the
non-overlapping template randomly, so that test may vary between runs.

## Project layout

- `stream_cipher.py`: LFSR transition, keystream generation, and XOR encryption.
- `web/cipher.js`: matching JavaScript implementation for browser decryption.
- `cipher_variants.py`: nonlinear feedback and Geffe generators.
- `stream_server.py`: encrypted video range API.
- `tools/`: performance and statistical evaluation.
- `tests/`: cipher, server, browser, and evaluation checks.
