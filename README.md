## Responsibility

Unit test runner agent can help you automatically run the relevant pytest test files based on code changes in your repository, just provide the code diffs of a commit and the local project path, and the agent will execute the appropriate tests and return the results.

## Details

* Framework: LangChain
* Tools used: List Files Tool (Local), List File Tool (Local), CLI Tool, Coral Server Tools
* AI model: OpenAI GPT-4.1
* Date added: 02/05/25
* Licence: MIT

## Use the Agent

### 1.Clone & Install Dependencies

Run [Interface Agent](https://github.com/Coral-Protocol/Coral-Interface-Agent)
<details>


If you are trying to run Open Deep Research agent and require an input, you can either create your agent which communicates on the coral server or run and register the Interface Agent on the Coral Server. In a new terminal clone the repository:


```bash
git clone https://github.com/Coral-Protocol/Coral-Interface-Agent.git
```
Navigate to the project directory:
```bash
cd Coral-Interface-Agent
```

Install `uv`:
```bash
pip install uv
```
Install dependencies from `pyproject.toml` using `uv`:
```bash
uv sync
```

Configure API Key
```bash
export OPENAI_API_KEY=
```

Run the agent using `uv`:
```bash
uv run python 0-langchain-interface.py
```

</details>

Agent Installation

<details>

Clone the repository:
```bash
git clone https://github.com/Coral-Protocol/Coral-UnitTestRunner-Agent.git
```

Navigate to the project directory:
```bash
cd Coral-UnitTestRunner-Agent
```

Install `uv`:
```bash
pip install uv
```

Install dependencies from `pyproject.toml` using `uv`:
```bash
uv sync
```

This command will read the `pyproject.toml` file and install all specified dependencies in a virtual environment managed by `uv`.

Copy the client sse.py from utils to mcp package
```bash
cp -r utils/sse.py .venv/lib/python3.10/site-packages/mcp/client/sse.py
```

OR Copy this for windows
```bash
cp -r utils\sse.py .venv\Lib\site-packages\mcp\client\sse.py
```

</details>

### 2. Configure Environment Variables

<details>

Copy the example file and update it with your credentials:

```bash
cp .env.example .env
```

Required environment variables:

* `OPENAI_API_KEY`

* **OPENAI_API_KEY:**
  Sign up at [platform.openai.com](https://platform.openai.com/), go to “API Keys” under your account, and click “Create new secret key.”

</details>

### 3. Run Agent

<details>

Unit test runner agent is supposed to receive local repo path and changed files as inputs, so please also run [Git clone agent](https://github.com/Coral-Protocol/Coral-GitClone-Agent) and [Code diffs review agent](https://github.com/Coral-Protocol/Coral-CodeDiffReview-Agent) to get proper inputs.

Run the agent using `uv`:
```bash
uv run 3-langchain-UnitTestRunnerAgent.py
```
</details>

### 4. Example

<details>

Input:

```bash
#Send message to the interface agent:
Please execute the unit test for the '2' PR in repo 'renxinxing123/camel-software-testing'.
```

Output:

```bash
Relevant test file: test/toolkits/test_semantic_scholar_functions.py

**Test Results:**
- Total tests: 11
- Passed: 9
- Failed: 2

**Failed tests:**
1. test_fetch_paper_data_id_success
2. test_fetch_paper_data_id_failure

**Summary:**
- The recent changes in `camel/toolkits/semantic_scholar_toolkit.py` caused failures in tests related to fetching paper data by ID. The function now returns `{'wrong_key': 'wrong_value'}` instead of the expected result or error dictionary.

**Pytest Output:**
============================= test session starts ==============================
platform linux -- Python 3.10.16, pytest-8.3.5, pluggy-1.5.0
rootdir: /home/xinxing/coraliser-/coral_examples/github-repo-understanding+unit_test_advisor/camel-software-testing
configfile: pyproject.toml
plugins: anyio-4.9.0, Faker-19.13.0, langsmith-0.3.42
collected 11 items

test/toolkits/test_semantic_scholar_functions.py ....FF.....             [100%]

=================================== FAILURES ===================================
_________ TestSemanticScholarToolkit.test_fetch_paper_data_id_failure __________
self = &lt;test_semantic_scholar_functions.TestSemanticScholarToolkit testMethod=test_fetch_paper_data_id_failure&gt;
mock_get = &lt;MagicMock name='get' id='134648639737472'&gt;
&gt;       self.assertIn(&quot;error&quot;, response)
E       AssertionError: 'error' not found in {'wrong_key': 'wrong_value'}

test/toolkits/test_semantic_scholar_functions.py:110: AssertionError
_________ TestSemanticScholarToolkit.test_fetch_paper_data_id_success __________
self = &lt;test_semantic_scholar_functions.TestSemanticScholarToolkit testMethod=test_fetch_paper_data_id_success&gt;
mock_get = &lt;MagicMock name='get' id='134648640270064'&gt;
&gt;       self.assertEqual(response, mock_response_data)
E       AssertionError: {'wrong_key': 'wrong_value'} != {'title': 'Paper Title by ID'}
E       - {'wrong_key': 'wrong_value'}
E       + {'title': 'Paper Title by ID'}

test/toolkits/test_semantic_scholar_functions.py:87: AssertionError
=========================== short test summary info ============================
FAILED test/toolkits/test_semantic_scholar_functions.py::TestSemanticScholarToolkit::test_fetch_paper_data_id_failure
FAILED test/toolkits/test_semantic_scholar_functions.py::TestSemanticScholarToolkit::test_fetch_paper_data_id_success
=================== 2 failed, 9 passed, 5 warnings in 1.49s ====================
```

</details>

## Creator details

* Name: Xinxing
* Affiliation: Coral Protocol
* Contact: [Discord](https://discord.com/invite/Xjm892dtt3)
