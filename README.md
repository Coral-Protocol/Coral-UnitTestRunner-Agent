## [Coral UnitTestRunner Agent](https://github.com/Coral-Protocol/Coral-UnitTestRunner-Agent)

The UnitTestRunner Agent can help you automatically run the relevant pytest test files based on code changes in your repository. Just provide the code diffs of a commit and the local project path, and the agent will execute the appropriate tests and return the results.

## Responsibility
The UnitTestRunner Agent automates running relevant unit tests for changed files in your repository, streamlining the testing process after code changes.

## Details
- **Framework**: LangChain
- **Tools used**: List Files Tool (Local), List File Tool (Local), CLI Tool, Coral Server Tools
- **AI model**: OpenAI GPT-4.1
- **Date added**: 02/05/25
- **License**: MIT

## Use the Agent  

### 1. Clone & Install Dependencies


<details>  

Ensure that the [Coral Server](https://github.com/Coral-Protocol/coral-server) is running on your system and the [Interface Agent](https://github.com/Coral-Protocol/Coral-Interface-Agent) is running on the Coral Server.  

```bash
# Clone the UnitTestRunner Agent repository
git clone https://github.com/Coral-Protocol/Coral-UnitTestRunner-Agent.git

# Navigate to the project directory
cd Coral-UnitTestRunner-Agent

# Install `uv`:
pip install uv

# Install dependencies from `pyproject.toml` using `uv`:
uv sync
```
This command will read the `pyproject.toml` file and install all specified dependencies in a virtual environment managed by `uv`.

</details>

### 2. Configure Environment Variables
<details>

Get the API Key:
- [OpenAI API Key](https://platform.openai.com/api-keys)

Create a .env file in the project root:
```bash
cp -r .env.example .env
```

Add your API keys and any other required environment variables to the .env file.

Required environment variables:
- `OPENAI_API_KEY`




</details>

### 3. Run Agent
<details>

UnitTestRunner Agent is supposed to receive local repo path and changed files as inputs, so please also run [GitClone Agent](https://github.com/Coral-Protocol/Coral-GitClone-Agent) and [CodeDiffReview Agent](https://github.com/Coral-Protocol/Coral-CodeDiffReview-Agent) to get proper inputs.

Run the agent using `uv`:
```bash
uv run 3-langchain-UnitTestRunnerAgent.py
```
</details>

### 4. Example
<details>

```bash
# Input:
Could you please execute the unit test for changed file 'semantic_scholar_toolkit.py' in the local path 'camel-software-testing'

# Output:
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
self = <test_semantic_scholar_functions.TestSemanticScholarToolkit testMethod=test_fetch_paper_data_id_failure>
mock_get = <MagicMock name='get' id='134648639737472'>
>       self.assertIn("error", response)
E       AssertionError: 'error' not found in {'wrong_key': 'wrong_value'}

test/toolkits/test_semantic_scholar_functions.py:110: AssertionError
_________ TestSemanticScholarToolkit.test_fetch_paper_data_id_success __________
self = <test_semantic_scholar_functions.TestSemanticScholarToolkit testMethod=test_fetch_paper_data_id_success>
mock_get = <MagicMock name='get' id='134648640270064'>
>       self.assertEqual(response, mock_response_data)
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

## Creator Details
- **Name**: Xinxing
- **Affiliation**: Coral Protocol
- **Contact**: [Discord](https://discord.com/invite/Xjm892dtt3)
