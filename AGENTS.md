# Trading Autoresearch Project Rules

## Mission
Build an autonomous research harness for systematic trading strategies.
This project is for OFFLINE strategy research, backtesting, and paper trading.
Do not create self-modifying live trading logic.

## Core principles
- Keep the evaluator frozen and deterministic.
- The strategy surface must stay narrow and reviewable.
- Prefer simple, testable code over abstractions.
- Minimize dependencies.
- Every result must be reproducible from code + config + data manifest.
- Never optimize on the final holdout set.
- Never edit live execution code as part of research experiments.
- Never weaken tests or validation gates to make a strategy pass.

## Editable vs frozen boundaries
Codex may edit:
- src/strategies/strategy.py
- src/strategies/features.py
- src/strategies/params.py
- configs/*.yaml
- tests/*

Codex must NOT edit unless explicitly asked:
- src/evaluator/*
- src/execution/*
- src/risk/*
- data/manifests/*
- promotion rules
- cost model
- slippage model
- walk-forward split definitions

## Technical requirements
- Python 3.11
- Use pandas, numpy, pydantic, pyyaml, pyarrow, pytest, matplotlib
- Keep functions small and explicit
- Add type hints for public functions
- Add docstrings for non-trivial modules
- No hidden global state
- No notebook-only logic in core modules

## Backtest rules
- Support daily and intraday candle strategies
- Include brokerage, taxes/fees, slippage, and spread assumptions
- No lookahead bias
- No survivorship bias in universe construction where avoidable
- Handle gaps and missing bars safely
- Position sizing must be explicit
- Portfolio constraints must be explicit
- Log every trade with reason, timestamp, price, size, fees

## Evaluation rules
Promotion requires:
- positive out-of-sample score
- acceptable max drawdown
- minimum trade count
- strategy survives higher slippage stress
- parameter neighborhood is reasonably stable
- no single year or symbol dominates results excessively

## Delivery rules
For every task:
1. explain the plan briefly
2. inspect repo structure before editing
3. make the smallest coherent set of changes
4. run tests / linters relevant to the task
5. summarize files changed, commands run, and any open issues

## If blocked
- Do not guess silently
- Leave a clear TODO with the exact missing assumption
- Prefer partial working scaffolds over large speculative rewrites
