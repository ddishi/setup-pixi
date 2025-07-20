# Requires-Pixi Integration Test

This directory contains examples demonstrating the new `requires-pixi` integration functionality.

## Test Cases

### 1. pixi.toml with requires-pixi
```toml
[project]
name = "test-requires-pixi"
channels = ["conda-forge"]
platforms = ["linux-64", "linux-aarch64", "osx-64", "osx-arm64", "win-64"]
requires-pixi = "v0.44.0"  # This version will be automatically used

[dependencies]
python = "3.11.*"
```

### 2. pyproject.toml with requires-pixi
```toml
[tool.pixi.project]
name = "test-requires-pixi-pyproject"
channels = ["conda-forge"]
platforms = ["linux-64", "linux-aarch64", "osx-64", "osx-arm64", "win-64"]
requires-pixi = "v0.45.0"  # This version will be automatically used

[tool.pixi.dependencies]
python = "3.11.*"
```

## Behavior

### When no explicit pixi-version is provided:
1. Action reads the `requires-pixi` field from manifest file
2. Uses that version to download pixi
3. Falls back to "latest" if no `requires-pixi` field exists

### When explicit pixi-version is provided:
1. Action uses the explicit version (ignores `requires-pixi`)
2. Maintains full backward compatibility

### Error handling:
- Invalid `requires-pixi` values show a warning and fall back to 'latest'
- Missing manifest files are handled gracefully
- Action continues to work as before when no `requires-pixi` is specified

## Usage

With this change, users can now specify the pixi version directly in their manifest files:

```yaml
# No need to specify pixi-version anymore!
- uses: prefix-dev/setup-pixi@v0.9.0
  with:
    cache: true
```

The action will automatically read the `requires-pixi` field and use the specified version.