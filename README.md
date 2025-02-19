# IBC-Lightning

![python](https://img.shields.io/badge/python-3.10-blue)
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json)](https://github.com/astral-sh/uv)
[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/charliermarsh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)
[![CI](https://github.com/nomutin/IBC-Lightning/actions/workflows/ci.yaml/badge.svg)](https://github.com/nomutin/IBC-Lightning/actions/workflows/ci.yaml)
[![codecov](https://codecov.io/gh/nomutin/IBC-Lightning/graph/badge.svg?token=K8TYB80GEE)](https://codecov.io/gh/nomutin/IBC-Lightning)

Lightning implementation of [Implicit Behavioral Cloning](https://arxiv.org/abs/2109.00137).

## Installation

```bash
pip install git+https://github.com/nomutin/IBC-Lightning.git
```

## API

```python
from torch import randn, nn
from ibc_lightning import IBC

states, actions = randn(10, 50, 64), randn(10, 50, 2)

ibc = IBC(
    state_encoder=nn.Linear(64, 16),
    energy_head=nn.Linear(16 + 2, 1),
    lower_bounds=(-1.0, -1.0),
    upper_bounds=(+1.0, +1.0),
)

# Predict energy
energy = ibc.forward(states=states, actions=actions)

# Predict action (derivative-free optimization)
action = ibc.inference(state=states[:, -1, :])
```

## Example

Training script is in [./example/](example/).

```bash
uv sync --extra train
poe train fit --config examples/config.yaml --trainer.devices [x]
```

## References

```bibtex
@software{zakka2021ibc,
    author = {Zakka, Kevin},
    month = {10},
    title = {{A PyTorch Implementation of Implicit Behavioral Cloning}},
    url = {https://github.com/kevinzakka/ibc},
    version = {0.0.1},
    year = {2021}
}
```

```bibtex
@misc{florence2021implicit,
    title = {Implicit Behavioral Cloning},
    author = {Pete Florence and Corey Lynch and Andy Zeng and Oscar Ramirez and Ayzaan Wahid and Laura Downs and Adrian Wong and Johnny Lee and Igor Mordatch and Jonathan Tompson},
    year = {2021},
    eprint = {2109.00137},
    archivePrefix = {arXiv},
    primaryClass = {cs.RO}
}
```
