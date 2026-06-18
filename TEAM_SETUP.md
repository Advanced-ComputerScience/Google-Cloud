# DoubleChat — Team Setup & Workflow

This is the shared base of the project. Everyone starts from this, then works
on their own feature on a separate branch.

## 1. One-time local setup

```bash
# create and activate a virtual environment
python -m venv venv
.\venv\Scripts\Activate.ps1        # Windows
# source venv/bin/activate          # macOS / Linux

# install dependencies
pip install -r requirements.txt

# create your own secrets file (NEVER commit .env)
copy .env.example .env             # then edit .env with the real values

# authenticate Google Cloud (one time per machine)
gcloud auth application-default login
gcloud auth application-default set-quota-project <project-id>

# verify everything works, then run
python check_setup.py
streamlit run app.py
```

> You each need your own `.env` and your own `gcloud` login. Secrets are
> gitignored and are NOT in this zip.

## 2. Branch workflow (this is how we avoid conflicts)

```bash
git clone <shared-repo-url>
cd <repo>
git checkout -b <yourname>-<feature>      # e.g. nandan-compare
# ... edit ONLY your own files ...
git add .
git commit -m "describe what you changed"
git push origin <yourname>-<feature>
# then open a Pull Request on GitHub -> Chandan reviews & merges
```

## 3. Who owns which files

| Feature | Person | Your files (edit only these) |
|---|---|---|
| Compare | Nandan | `views/compare.py` |
| Benchmark | Nandan | `views/benchmark.py`, `benchmark/` |
| Deployment | Nandan | (Cloud Run config — to be added) |
| Capabilities | Madhusha | `views/capabilities.py` |
| Code generation | Madhusha | `views/code_gen.py` |
| Chat | Chandan | `views/chat.py` |
| History | Chandan | `views/history.py`, `storage/` |
| Fine Tuning | Chandan | `views/finetune.py`, `finetuning/` |

## 4. The golden rule (prevents conflicts)

- ✅ Edit **only your own files** from the table above — these never conflict.
- ⚠️ **Do NOT edit these shared files** without telling Chandan (the lead):
  - `app.py` (page navigation)
  - `config.py` (settings)
  - `chatbots/` (the Gemini + Claude wrappers everyone uses)
  - `requirements.txt`

If your feature needs something added to a shared file, ask Chandan to do it,
so only one person ever edits those files.
