# Lizard Runner

[![pyt](https://github.com/Uno-Takashi/lizard-runner/actions/workflows/pyt.yml/badge.svg?branch=main&event=push)](https://github.com/Uno-Takashi/lizard-runner/actions/workflows/pyt.yml)
[![Lizard Runner](https://github.com/Uno-Takashi/lizard-runner/actions/workflows/lizard.yml/badge.svg?branch=main&event=push)](https://github.com/Uno-Takashi/lizard-runner/actions/workflows/lizard.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/Uno-Takashi/lizard-runner/blob/main/LICENSE)
[![Contributors](https://img.shields.io/github/contributors/Uno-Takashi/lizard-runner)](https://github.com/Uno-Takashi/lizard-runner/graphs/contributors)
[![Python](https://img.shields.io/badge/Python-F9DC3E.svg?logo=python&style=flat)](https://www.python.org/)
[![Docker](https://img.shields.io/badge/-Docker-EEE.svg?logo=docker&style=flat)](https://www.docker.com/)

Lizard Runner is a third-party OSS Github Action for easy execution of [lizard](http://www.lizard.ws/).

- **Rapid Container**
  - The Lizard Runner employs a lightweight container to achieve build times that are competitive with the combination of actions/setup-python and pip. In most cases, preparation to begin analysis takes less than 30 seconds.
- **Easy-to-use variable specification**
  - Wrapping command line arguments allows for more intuitive execution.
- **Can be embedded in CI**
  - The complexity of the code can always be measured. This prevents the inclusion of overly complex codes.

## 🗞️ Introduction

As code becomes more complex, it becomes less maintainable.

Often, code that was easy to modify in the early stages of a product's life can become so complex and huge before long that it becomes unmaintainable.

By using the lizard-runner, engineers can keep track of the complexity of their code.

## 🛠️ Why We Built It

In general, CI allows engineers to gain psychological security by keeping test code running. That is, they continue to be assured that the code they add will not have a fatal negative impact on the service.

We wanted to incorporate and visualize code complexity within the CI framework. By doing this, we wanted to make it more psychologically safe for engineers to merge code, ensuring that the code they wrote would not become a bottleneck in the refurbishment.

For this reason, we have created a Github action that makes it easy to run lizard!

## 📚 Usage

The following code is the minimum code to make Lizard-Runner work. In the following cases, lizard is run with default settings.

```yml
name: Lizard Runner
on:
  push:

jobs:
  lizard:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Lizard Runner
        uses: Uno-Takashi/lizard-runner@v1
```

lizard execution options can be passed as arguments. For example, the case for setting up CCN and execution paths is as follows.

```yml
      - name: Lizard Runner
        uses: Uno-Takashi/lizard-runner@v1
        with:
          path: "./src ./libs"
          CCN: "20"
```

See "Inputs" for a detailed description of all the arguments that can be set.

## 📥 Inputs

Most of the input is the same as in the [lizard](http://www.lizard.ws/), but some original input is required for lizard-runner. Some arguments are also wrapped for convenience.

## 📤 Outputs

The paths to the two files are output.
The following outputs can be accessed via ${{ steps.<step-id>.outputs }} from this action

|       **Name**       |                                  **Description**                                 |
|:--------------------:|:--------------------------------------------------------------------------------:|
| `cli_output_path`    | The path to the file where the output result of cli is saved when lizard is run. |
| `result_output_path` | Output only if "input_file" is given. lizard's output_file path.                 |

### See outputs

```yml
      - name: test-action
        uses: ./ # Uses an action in the root directory
        id: lizard
      - name: Get result_output_file
        run: echo -e "${{ steps.lizard.outputs.result_output_path }}"
      - name: Get cli_output_file
        run: echo -e "${{ steps.lizard.outputs.cli_output_path }}"
      - name: Archive lizard log
        uses: actions/upload-artifact@v2
        with:
          name: "lizard_cli_output"
          path: "${{ steps.lizard.outputs.cli_output_path }}"
```

## 🛒 How to Get

[Lizard Runner · Actions · GitHub Marketplace](https://github.com/marketplace/actions/lizard-runner)

## ⌨ Development
