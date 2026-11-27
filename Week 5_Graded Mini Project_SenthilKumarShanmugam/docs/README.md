# Running `mp1_prompt_lab.ipynb`

This guide explains how to set up and run the prompt-lab notebook in VS Code on Windows.

## 1. Prepare Python and install dependencies

Open PowerShell in the project root (the folder containing `requirements.txt`) and create an environment:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Python 3.10 or later is required. In VS Code, open `learner/mp1_prompt_lab.ipynb`, choose the `.venv` Python interpreter as its kernel, and install the Python and Jupyter extensions if prompted.

## 2. Configure the OpenAI API key

The notebook sends requests to `gpt-4o-mini` and `gpt-4o`. Set `OPENAI_API_KEY` as a Windows user environment variable, then fully restart VS Code and the notebook kernel so the kernel receives it. Do not paste the key into the notebook or commit it to source control.

The first setup cell checks for this variable. If it raises `Set OPENAI_API_KEY first`, the selected kernel cannot see the key.

## 3. Run the notebook

Run the code cells from top to bottom:

1. **Setup** — imports dependencies and creates the OpenAI client.
2. **Step 1** — loads `job_snippets.jsonl` and `golden_set.jsonl`.
3. **Step 2** — defines the four prompting strategies.
4. **Step 3** — calls `run_all()` to run 10 snippets across 4 strategies. It should return 40 results.
5. **Step 4** — calculates exact-field accuracy and parse success, then asks `gpt-4o` to judge each result.
6. **Step 5** — displays the comparison table.

The notebook sets `DATA_DIR = Path('../data')`, which assumes its working directory is `learner/`. If the notebook reports that it cannot find the JSONL files, check the working directory. When it is the project root instead, change the setting to `Path('data')`.

## 4. Understand API usage

Running Step 3 makes 40 `gpt-4o-mini` calls. Running Step 4 makes another 40 `gpt-4o` judge calls. These calls may incur charges and are not needed just to edit or inspect the notebook. Check your API account's model access and usage limits before running them.

## 5. Results and troubleshooting

- Step 3 prints `Got 40 results.` and displays one result, including the raw response, parsed extraction, cost, and latency.
- Step 5 displays the average accuracy, parse rate, judge score, total cost, and median latency for each strategy.
- If `pandas` or `openai` cannot be imported, confirm that VS Code is using `.venv`, then run `python -m pip install -r requirements.txt` in that environment.
- If data files cannot be found, correct the notebook working directory or `DATA_DIR` as described above.
- If a model request fails, verify that the API key is available to the kernel and that the account can access the requested model.

The notebook uses top-level `await`, so run it in a Jupyter notebook kernel rather than as a regular Python script.
