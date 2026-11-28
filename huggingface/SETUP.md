# GitHub to Hugging Face dataset sync

Source: https://github.com/Double-FW/Cosmos-UX-Design  
Destination: https://huggingface.co/datasets/GarethFW/Cosmos_UX_Design

This is a one-way export: GitHub is the authoring source and the workflow publishes only to the named Hugging Face dataset. The exporter uses an explicit allowlist in `huggingface/export-config.json`; it includes selected guidance documents and `LICENSE`. The Component Semantics Contract and its companion guides on ARIA and visually hidden content are exported as full documents and heading-based sections. It does not fetch linked pages, issues, discussions, or external repositories.

## One-time owner setup

1. Create a fine-grained Hugging Face token with read and write access to only `GarethFW/Cosmos_UX_Design`. The dataset repository must already exist.
2. In GitHub, open **Settings → Secrets and variables → Actions → New repository secret**.
3. Set **Name** to `HF_TOKEN`.
4. Paste the token into **Secret** and click **Add secret**.
5. Confirm the sync setup is on the default `main` branch.
6. Open **Actions → Sync Cosmos UX Design dataset → Run workflow**, select `main`, then click **Run workflow**.
7. Confirm the run succeeds and inspect the Hugging Face dataset's **Files and versions** page.

Add the secret before merging the setup PR: merging adds the workflow to `main`, and the push trigger can start the first sync immediately. Do not put the token in a file, issue, commit, workflow log, or chat.

## Ongoing behaviour

- A push to `main` that changes an allowlisted source file, the exporter, configuration, requirements, setup guide or workflow triggers a sync.
- A scheduled sync runs each Monday at 07:23 UTC. GitHub may delay scheduled runs.
- A manual workflow run is available. Pull requests do not publish; publishing is restricted to the default branch.
- The destination is fixed in both workflow and exporter. The publisher verifies the public GitHub source and MIT licence, checks that the existing dataset repository is accessible, and fails before writing if the destination is missing or uninitialised.
- Only these dataset files are managed: `README.md` (dataset card), `LICENSE`, `documents.jsonl`, `knowledge_chunks.jsonl`, `provenance.json`, `record_schema.json`, and `export_manifest.json`. Unrelated destination files and dataset visibility are preserved.
- The full-document and section configurations overlap by design. Documents contain every allowlisted Markdown file; section records contain all allowlisted Markdown guidance except `README.md` and `AGENTS.md`.
- If managed files have not changed, no Hugging Face commit is created.

If a run fails, inspect its job log. Common causes include a missing or incorrectly scoped token, destination access problems, source visibility or licence changes, or a concurrent destination update. Do not share credentials in issues, logs, or chat.
