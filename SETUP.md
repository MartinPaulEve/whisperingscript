# Setup Guide for WhisperingScript

This guide explains how to set up, test, and publish the WhisperingScript package.

## Development Setup

### 1. Create Virtual Environment

```bash
# Install python3-venv if not available (Ubuntu/Debian)
sudo apt install python3.12-venv

# Create virtual environment
python3 -m venv .venv

# Activate virtual environment
source .venv/bin/activate

# Upgrade pip
pip install --upgrade pip
```

### 2. Install Package in Development Mode

```bash
# Install package in editable mode with development dependencies
pip install -e ".[dev]"

# Or install just the package
pip install -e .
```

### 3. Test Installation

```bash
# Test command-line interface
whisperingscript --help
whispering --help

# Test Python API
python -c "from whisperingscript import WhisperingAutomation; print('Import successful')"
```

## Building the Package

### 1. Install Build Tools

```bash
pip install build twine
```

### 2. Build Distribution

```bash
# Clean previous builds
rm -rf dist/ build/ *.egg-info/

# Build source and wheel distributions
python -m build
```

This creates:
- `dist/whisperingscript-0.1.0.tar.gz` (source distribution)
- `dist/whisperingscript-0.1.0-py3-none-any.whl` (wheel distribution)

### 3. Test Built Package

```bash
# Create fresh virtual environment for testing
python3 -m venv test_env
source test_env/bin/activate

# Install from built wheel
pip install dist/whisperingscript-0.1.0-py3-none-any.whl

# Test installation
whisperingscript --version
```

## Publishing to PyPI

### 1. Test on TestPyPI First

```bash
# Upload to TestPyPI
twine upload --repository testpypi dist/*

# Test installation from TestPyPI
pip install --index-url https://test.pypi.org/simple/ whisperingscript
```

### 2. Publish to PyPI

```bash
# Upload to PyPI
twine upload dist/*
```

### 3. Verify Publication

```bash
# Install from PyPI
pip install whisperingscript

# Test functionality
whisperingscript --help
```

## Prerequisites for Users

### System Requirements

1. **Python 3.8+**
2. **Chrome Browser**
3. **ChromeDriver** (must be in PATH)

### Installing ChromeDriver

#### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install chromium-chromedriver
```

#### macOS
```bash
brew install chromedriver
```

#### Windows
Download from [ChromeDriver Downloads](https://chromedriver.chromium.org/) and add to PATH.

### OpenAI API Key Setup

Create a file at `~/.openai` containing your OpenAI API key:

```bash
echo "your-openai-api-key-here" > ~/.openai
chmod 600 ~/.openai  # Secure the file
```

## Testing the Package

### Manual Testing

1. **Basic functionality test:**
   ```bash
   whisperingscript --stop-method time --duration 5 --no-headless
   ```

2. **API test:**
   ```python
   from whisperingscript import WhisperingAutomation
   
   automation = WhisperingAutomation(headless=False, stop_method="time", recording_duration=5)
   # Test individual methods
   automation.setup_browser()
   automation.navigate_to_site()
   automation.cleanup()
   ```

### Automated Testing

```bash
# Run tests (when implemented)
pytest

# Run with coverage
pytest --cov=whisperingscript

# Type checking
mypy src/

# Code formatting
black src/
```

## Version Management

To release a new version:

1. Update version in `pyproject.toml`
2. Update version in `src/whisperingscript/__init__.py`
3. Update version in `src/whisperingscript/main.py`
4. Update `CHANGELOG.md` (when created)
5. Build and publish new version

## Troubleshooting

### Common Issues

1. **ChromeDriver not found:**
   - Ensure ChromeDriver is installed and in PATH
   - Test with: `chromedriver --version`

2. **Permission errors:**
   - Use virtual environment instead of system Python
   - Check file permissions for API key file

3. **Import errors:**
   - Ensure package is installed: `pip list | grep whisperingscript`
   - Check Python path: `python -c "import sys; print(sys.path)"`

4. **Selenium issues:**
   - Update Chrome browser to latest version
   - Update ChromeDriver to match Chrome version
   - Check for conflicting browser processes

## Project Structure

```
whisperingscript/
├── src/
│   └── whisperingscript/
│       ├── __init__.py          # Package initialization
│       ├── automation.py        # Main automation class
│       └── main.py             # CLI entry point
├── pyproject.toml              # Project configuration
├── README.md                   # User documentation
├── LICENSE                     # MIT license
├── MANIFEST.in                 # Distribution files
├── requirements.txt            # Dependencies
├── .gitignore                 # Git ignore rules
└── SETUP.md                   # This file
```

## Next Steps

1. Test the package thoroughly in different environments
2. Add unit tests for key functionality
3. Set up CI/CD pipeline (GitHub Actions)
4. Create comprehensive documentation
5. Add logging and better error handling
6. Consider adding configuration file support
