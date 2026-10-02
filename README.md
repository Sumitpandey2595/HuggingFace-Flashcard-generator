# Hugging Face Flashcard Generator

## Objective

Generate exactly five concise study flashcards for a topic using a Hugging Face model hosted by an Inference Provider. Run the same prompt with two models and compare their outputs and response times.

## Requirements

- Python 3.10 or newer
- VS Code
- A Hugging Face account
- A Hugging Face access token with Inference Providers permission

## Project Structure

```text
huggingface_flashcards/
|-- app.py
|-- requirements.txt
|-- .env
|-- .env.example
|-- .gitignore
`-- README.md
```

## Installation

Open this project folder in VS Code. In its PowerShell terminal, run:

```powershell
python --version
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

If PowerShell blocks activation, allow it just for the current terminal session, then activate the environment:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.venv\Scripts\Activate.ps1
```

This process-scoped setting ends when that PowerShell window closes. If your computer is managed by an organization and policy prevents this command, follow your organization's guidance.

## Environment Setup

Create a Hugging Face access token from [Hugging Face token settings](https://huggingface.co/settings/tokens/new?tokenType=fineGrained&ownUserPermissions=inference.serverless.write). Give it the Inference Providers permission. The Gemma model may also require accepting its model license on its Hugging Face page.

The project includes a local `.env` file. Replace its placeholder with your own token:

```dotenv
HF_TOKEN=your_token_here
```

Use your real token only in the local `.env` file or as an environment variable; never paste it into source code. Do not upload your HF_TOKEN to GitHub, screenshots, chats, or shared files. `.env` is ignored by Git; `.env.example` is safe to share and contains only the placeholder.

## Run

With the virtual environment activated and dependencies installed:

```powershell
python app.py
```

The app asks `Enter a topic:` and prints the model ID, response time, and generated flashcards together for easy copying into your comparison table.

## Example Topic

```text
Machine Learning
```

## Model 1

The default model is `openai/gpt-oss-120b`. Run `python app.py`, enter a topic, and record the model ID, response time, and your observations. Hugging Face currently lists this model for chat completion through live Inference Providers.

## Model 2

The second model is `google/gemma-2-2b-it`. In `app.py`, find the marked **CHANGE ONLY THIS VALUE FOR MODEL COMPARISON** section and change:

```python
MODEL_ID = MODEL_1
```

to:

```python
MODEL_ID = MODEL_2
```

Then run `python app.py` again with the same topic. The system instruction, topic prompt, temperature, token limit, and formatting requirements remain unchanged. Hugging Face currently lists Gemma for chat completion through a live provider; access to its model license may be required. Both model IDs were checked against Hugging Face's current [chat-completion recommendations](https://huggingface.co/docs/inference-providers/tasks/chat-completion) and live provider mappings.

## Latency Measurement

The app records `time.perf_counter()` immediately before and after the Hugging Face request and displays the elapsed time in seconds to two decimal places. The measurement includes time spent waiting for the model/provider response, not a token-usage measurement.

## Comparison Table

Fill qualitative observations only after you have run both models. No response times or qualitative results are assumed here.

| Observation | Model 1 | Model 2 |
|------------|---------|---------|
| Model ID | `openai/gpt-oss-120b` | `google/gemma-2-2b-it` |
| Response time |  |  |
| Tone |  |  |
| Answer length |  |  |
| Format adherence |  |  |
| Other observations |  |  |

## Quick Reflection

1. **What is a Hugging Face Model ID?**  
   It is the unique name used to identify a model on Hugging Face, usually written as `owner/model-name`.

2. **What does InferenceClient do?**  
   It sends requests to a hosted model and returns the model's response, so this project does not download or train a model locally.

3. **What changed when you switched models?**  
   The model generating the flashcards changed, so its response style, content, and response time may differ.

4. **What stayed the same?**  
   The topic, system instruction, prompt, settings, and requested flashcard format stayed the same.

5. **What did you observe about token usage and latency?**  
   Token usage depends on the prompt and generated text. This app measures request latency but does not report token counts; use the observed response and displayed time when comparing the runs.

## Troubleshooting

- **HF_TOKEN missing:** Put your token in `.env` as `HF_TOKEN=your_token_here` (replacing the placeholder), save the file, and run the app again.
- **Invalid token or authentication error:** Check that the token is active and has Inference Providers permission. Do not paste the token into terminal commands that may be saved in shell history.
- **Model unavailable:** Check the exact model ID and its current provider mapping. Try again later or select another currently supported chat-completion model.
- **Provider unavailable:** The client uses `provider="auto"` to route to an available provider. If none is available, wait and retry; verify the model has a live provider mapping.
- **Network error or timeout:** Check your internet connection, then retry. The app displays a friendly message without printing the token or raw error details.
- **PowerShell virtual environment activation error:** Run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` in the current PowerShell window, then run `.venv\Scripts\Activate.ps1`.
- **Package not installed:** Activate `.venv` and run `pip install -r requirements.txt`. If `pip` is not on PATH, use `python -m pip install -r requirements.txt`.
