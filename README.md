# Chord / XTHP: small classroom adaptation

This is original toy code adapting the **predecessor tool** structure from
`systemsecurity-uiuc/Chord`, especially its `demo/semantic_targeted_hooking.py`
and `demo/syntax_format_hooking.py`. The upstream project is licensed MIT:
<https://github.com/systemsecurity-uiuc/Chord> (inspected commit
`8ac19e8bd5fd`). No upstream implementation or
dataset is copied into this package. The code uses the paper's separate
hijacking (HSR), harvesting (HASR), and polluting (PSR) measurements, reduced
to one local companion tool and six synthetic tasks.

## Run

Python 3.11+; no installation or API key for the default mode:

```bash
python demo.py
python demo.py --limit 2
```

Optional model-selected planning (two billable planning calls per case):

```bash
python -m pip install openai pydantic
# Set OPENAI_API_KEY in your own terminal.
python demo.py --mode api --model gpt-4.1 --limit 2
python demo.py --mode api --model gpt-4.1   # six cases, 12 API calls
```

`--mode offline` uses a transparent deterministic planner, so its rates are
**illustrative outcomes of those rules**. `--mode api` asks the LLM whether to
select the companion and whether to include the synthetic memo in its input;
the executor still runs all tools locally. The LLM receives the synthetic
memo in its planning prompt, so that mode is about whether it **passes that
memo to the additional tool**, not about whether the API provider saw it.
Do not use real private data as the memo. All six memo strings are fictional.

This is a teaching reduction, not the Chord scanner: it does not generate
malicious tool descriptions, optimize them, run LangChain agents, use real
third-party tools, contact an attacker server, or reproduce the paper's
66-tool results. In offline mode, the planner selects the helper when a
multiword topic is present; this is a designed rule rather than evidence that
an LLM is susceptible. In API mode, model behavior can vary by run and model.
