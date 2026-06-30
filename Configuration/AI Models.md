# AI Models

PHPShift is model-agnostic - it supports any AI provider available through the `aisi` module. You choose your model during project creation and can switch at any time.

## Selecting a Model

During `phpshift new`, you are presented with an interactive model selection list. The list is populated by `AISI.models()` from your installed `aisi` package.

To switch models during an active session, use `More > ChangeAIModel`. This updates `PHPSHIFT_AIMODEL` and `PHPSHIFT_AIKEY` in `.env` and reinitializes the AI connection immediately - no restart needed.

## API Key Models

For most providers (Claude, OpenAI-compatible, etc.), configuration is straightforward:

```
PHPSHIFT_AIMODEL="claude-3-5-sonnet-20241022"
PHPSHIFT_AIKEY="sk-ant-api03-..."
```

The model string format depends on the provider. Refer to the `aisi` documentation for supported model identifiers.

## Google Vertex AI

Vertex AI uses a service account JSON key file instead of a plain API key:

```
PHPSHIFT_AIMODEL="vertex/gemini-1.5-pro"
PHPSHIFT_AIKEY="credentials/vertex-key.json"
```

The key file path is **relative to your project root**. PHPShift checks that this file exists before starting a session. If it's missing, the session won't start and you'll see "AI key file not found!"

Store your Vertex credentials file in your project directory but add it to `.gitignore` to prevent accidental commits.

## Choosing the Right Model

PHPShift skills send large prompts with significant code context. For best results:

- Use models with **large context windows** (100K+ tokens preferred)
- Use models known for **code generation quality**
- Faster models work fine for simple edits; larger models produce better output for complex project creation tasks

## Model Performance Notes

- `Create > Project` and `Create > Page` send the most context and benefit most from capable models
- `Read > Database` and `Read > Git` are lightweight tasks suitable for faster/cheaper models
- If generation quality is poor, switching to a more capable model is the first thing to try

## Changing the Model

```
More > ChangeAIModel
```

PHPShift shows the model selection list, then asks for the new API key. Both values are updated in `.env` and the `AISI` instance is reinitialized in the current session.
