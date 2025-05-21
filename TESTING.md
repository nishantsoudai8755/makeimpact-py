# Testing the 1ClickImpact Python SDK

This document explains how to set up and run tests for the 1ClickImpact Python SDK.

## Prerequisites

1. Create and activate a virtual environment (recommended):

   ```bash
   # Create a virtual environment
   python3 -m venv venv

   # Activate it on macOS/Linux
   source venv/bin/activate

   # Activate it on Windows
   venv\Scripts\activate
   ```

2. Install pytest and other test dependencies (make sure your virtual environment is activated):

   ```bash
   # First make sure pip is up to date
   pip install --upgrade pip

   # Then install pytest and python-dotenv
   pip install pytest python-dotenv requests

   # Verify pytest was installed correctly
   python3 -c "import pytest; print(f'pytest version: {pytest.__version__}')"
   ```

3. Set up your API key:
   - Either set the `TEST_API_KEY` environment variable:
     ```bash
     export TEST_API_KEY=your_sandbox_api_key_here
     ```
   - Or create a `.env` file in the project root with:
     ```
     TEST_API_KEY=your_sandbox_api_key_here
     ```

## Running Tests

### Run all tests

```bash
# From the project root directory
python3 -m pytest tests/

# For more verbose output
python3 -m pytest -v tests/
```

If you receive "command not found: pytest", use the Python module format above or make sure pytest is installed in your active environment.

### Run specific test classes

```bash
# Run only the Plant Trees tests
python3 -m pytest tests/test_client.py::TestOneClickImpact::TestPlantTrees

# Run only the Clean Ocean tests
python3 -m pytest tests/test_client.py::TestOneClickImpact::TestCleanOcean
```

### Run specific test methods

```bash
# Run only the test for planting trees with a category
python3 -m pytest tests/test_client.py::TestOneClickImpact::TestPlantTrees::test_plant_trees_with_category
```

### Additional pytest options

```bash
# Show print statements during tests
python3 -m pytest -v tests/ -s

# Stop after first failure
python3 -m pytest tests/ -x

# Show detailed test summary
python3 -m pytest tests/ -v

# Generate test coverage report
# (requires pytest-cov package: pip install pytest-cov)
python3 -m pytest --cov=makeimpact tests/
```

## API Usage

The SDK uses a simple and intuitive keyword argument approach:

```python
# Simple API with direct keyword arguments
sdk.plant_tree(amount=1, category="reforestation")
sdk.get_records(filter_by="tree_planted", limit=10)
sdk.get_customer_records(customer_email="example@email.com")
```

## Notes

- Tests communicate with the 1ClickImpact sandbox API, so an internet connection is required.
- All tests use the sandbox environment to avoid affecting production data.
- If no API key is provided, tests will be skipped with a warning.

## Troubleshooting

- If you see "zsh: command not found: pytest", ensure that:

  1. You've activated your virtual environment
  2. You've installed pytest: `pip install pytest`
  3. Try running through the Python module: `python -m pytest`

- If you see the message "⚠️ No TEST_API_KEY environment variable found. Skipping live API tests.", check that:

  1. Your .env file is in the correct location (project root)
  2. The variable name is exactly `TEST_API_KEY`
  3. You've installed python-dotenv: `pip install python-dotenv`

- If tests fail with authorization errors, verify that your API key is valid for the sandbox environment.
