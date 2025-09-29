# Breast Cancer Receptor Extraction - To-Do List

## Phase 1: Package Setup (Week 1)

### Repository & Environment
- [ ] Initialize git repository (`git init`)
- [ ] Create GitHub repo and link it
- [ ] Set up virtual environment (✓ DONE)
- [ ] Create `.gitignore` file
- [ ] Choose and add LICENSE file (recommend MIT)

### Package Structure
- [ ] Create main package folder: `bc_receptor/`
- [ ] Add `__init__.py` files to all package folders
- [ ] Create subfolders: `extractors/`, `parsers/`, `reconciliation/`, `patterns/`, `utils/`
- [ ] Create `tests/` folder with `__init__.py`
- [ ] Create `examples/` folder
- [ ] Create `docs/` folder
- [ ] Create `config/` folder

### Configuration Files
- [ ] Create `setup.py` for pip installation
- [ ] Create `pyproject.toml`
- [ ] Create `requirements.txt` (pandas, numpy, regex, pyyaml)
- [ ] Create `requirements-dev.txt` (pytest, pytest-cov, black, flake8)
- [ ] Create basic `README.md` with project description

### Install Dependencies
- [ ] Activate virtual environment
- [ ] Install requirements: `pip install -r requirements.txt`
- [ ] Install dev requirements: `pip install -r requirements-dev.txt`
- [ ] Test pytest works: `pytest --version`

---

## Phase 2: Core Extraction Logic (Week 1-2)

### Regex Patterns
- [ ] Create `patterns/regex_library.py`
- [ ] Define ER positive/negative patterns
- [ ] Define ER percentage patterns (e.g., "ER 85%")
- [ ] Define PR positive/negative patterns
- [ ] Define PR percentage patterns
- [ ] Define HER2 IHC patterns (0, 1+, 2+, 3+)
- [ ] Define HER2 FISH patterns (amplified/not amplified)
- [ ] Test patterns on sample text

### Medical Synonyms
- [ ] Create `patterns/synonyms.py`
- [ ] Add ER synonym mappings (estrogen receptor, ER, etc.)
- [ ] Add PR synonym mappings (progesterone receptor, PR, etc.)
- [ ] Add HER2 synonym mappings (HER2, HER2/neu, ERBB2, etc.)

### Text Utilities
- [ ] Create `utils/text_utils.py`
- [ ] Write `clean_text()` function (lowercase, strip whitespace)
- [ ] Write `extract_context_window()` function (get surrounding text)
- [ ] Write `detect_negation()` function (check for "not", "negative", etc.)
- [ ] Add unit tests for text utilities

### ER Parser
- [ ] Create `parsers/er_parser.py`
- [ ] Write `parse_er_status()` function
- [ ] Extract positive/negative/unknown status
- [ ] Extract percentage if present
- [ ] Handle negation (e.g., "not ER positive")
- [ ] Return standardized dict: `{'status': 'positive', 'percentage': 85}`
- [ ] Add unit tests with 10+ example cases

### PR Parser
- [ ] Create `parsers/pr_parser.py`
- [ ] Write `parse_pr_status()` function (similar to ER)
- [ ] Extract positive/negative/unknown status
- [ ] Extract percentage if present
- [ ] Handle negation
- [ ] Return standardized dict
- [ ] Add unit tests with 10+ example cases

### HER2 Parser
- [ ] Create `parsers/her2_parser.py`
- [ ] Write `parse_her2_ihc()` function (extract 0/1+/2+/3+)
- [ ] Write `parse_her2_fish()` function (extract amplified/not amplified)
- [ ] Write `determine_her2_status()` function:
  - [ ] If IHC 0 or 1+ → negative
  - [ ] If IHC 3+ → positive
  - [ ] If IHC 2+ and FISH amplified → positive
  - [ ] If IHC 2+ and FISH not amplified → negative
  - [ ] If IHC 2+ without FISH → flag as "needs FISH"
- [ ] Return standardized dict: `{'status': 'positive', 'ihc': '3+', 'fish': None}`
- [ ] Add unit tests with 15+ example cases (including 2+ scenarios)

---

## Phase 3: Extractor Classes (Week 2-3)

### Base Extractor
- [ ] Create `extractors/base.py`
- [ ] Define `BaseExtractor` abstract class
- [ ] Define standard input format (text string)
- [ ] Define standard output format (dict with er/pr/her2)
- [ ] Add docstrings explaining the interface

### NLP Extractor
- [ ] Create `extractors/nlp.py`
- [ ] Create `NLPExtractor` class
- [ ] Write `extract_from_text(text)` method
- [ ] Call all three parsers (ER, PR, HER2)
- [ ] Combine results into single dict
- [ ] Add confidence scores if possible
- [ ] Test on 20+ sample pathology notes

### Structured Data Extractor
- [ ] Create `extractors/structured.py`
- [ ] Create `StructuredExtractor` class
- [ ] Write `extract_from_dataframe(df, field_mapping)` method
- [ ] Support common EHR field names (er_result, pr_result, etc.)
- [ ] Allow custom field mappings via config
- [ ] Handle missing values gracefully
- [ ] Test on sample DataFrame

### Main ReceptorExtractor Class
- [ ] Create main class in `bc_receptor/__init__.py`
- [ ] Write `extract_from_text(text)` method → calls NLPExtractor
- [ ] Write `extract_from_dataframe(df)` method → calls StructuredExtractor
- [ ] Write `extract_batch(texts)` method → batch processing
- [ ] Add logging throughout
- [ ] Allow custom patterns via constructor
- [ ] Add docstrings with examples

---

## Phase 4: Reconciliation & Validation (Week 3)

### Merger
- [ ] Create `reconciliation/merger.py`
- [ ] Write `merge_sources()` function
- [ ] Implement priority hierarchy (structured > NLP)
- [ ] Handle conflicting results (flag for review)
- [ ] Add voting mechanism if multiple NLP extractions
- [ ] Test on cases with conflicts

### Temporal Logic
- [ ] Create `reconciliation/temporal.py`
- [ ] Write `select_most_recent()` function
- [ ] Handle date/timestamp parsing
- [ ] Identify diagnostic vs recurrence biopsies
- [ ] Flag cases with changing receptor status over time

### Validator
- [ ] Create `reconciliation/validator.py`
- [ ] Write quality check functions:
  - [ ] `check_completeness()` - all 3 receptors present?
  - [ ] `check_consistency()` - ER+ but 0%? Flag it.
  - [ ] `check_her2_logic()` - HER2 2+ without FISH? Flag it.
  - [ ] `check_unusual_patterns()` - triple negative checks, etc.
- [ ] Return list of warnings/flags
- [ ] Test on edge cases

### Medical Logic
- [ ] Create `utils/medical_logic.py`
- [ ] Define standard clinical rules
- [ ] Add helper functions for common checks
- [ ] Document clinical decision logic

---

## Phase 5: Testing & Documentation (Week 3-4)

### Test Data
- [ ] Create `tests/fixtures/sample_notes.json` with 50+ synthetic examples
- [ ] Include edge cases: negation, unclear results, abbreviations
- [ ] Create `tests/fixtures/expected_outputs.json` with ground truth
- [ ] Create diverse test cases (different formats, EHR systems)

### Unit Tests
- [ ] Write `tests/test_er_parser.py` (>10 test cases)
- [ ] Write `tests/test_pr_parser.py` (>10 test cases)
- [ ] Write `tests/test_her2_parser.py` (>15 test cases, include 2+ logic)
- [ ] Write `tests/test_extraction.py` (integration tests)
- [ ] Aim for >80% code coverage
- [ ] Run: `pytest --cov=bc_receptor tests/`

### Validation on Real Data
- [ ] Collect 100-200 real pathology reports (de-identified)
- [ ] Manually review and create ground truth labels
- [ ] Run extraction algorithm on validation set
- [ ] Calculate performance metrics:
  - [ ] ER: sensitivity, specificity, PPV, NPV, accuracy
  - [ ] PR: sensitivity, specificity, PPV, NPV, accuracy
  - [ ] HER2: sensitivity, specificity, PPV, NPV, accuracy
- [ ] Document results in `benchmarks/performance_results.csv`
- [ ] Perform error analysis - categorize failure types
- [ ] Document in `benchmarks/error_analysis.md`
- [ ] Iterate on patterns/logic based on errors

### Documentation
- [ ] Write comprehensive `README.md`:
  - [ ] Project description and motivation
  - [ ] Installation instructions
  - [ ] Quick start example (3-5 lines of code)
  - [ ] Features list
  - [ ] Performance metrics from validation
  - [ ] Citation information
  - [ ] Link to documentation
- [ ] Create `docs/quickstart.md` - simple tutorial
- [ ] Create `docs/api_reference.md` - all classes/methods
- [ ] Create `docs/data_formats.md` - expected input/output formats
- [ ] Create `docs/customization.md` - how to add custom patterns
- [ ] Create `docs/validation_guide.md` - how users should validate
- [ ] Add docstrings to ALL public methods
- [ ] Add type hints to function signatures

---

## Phase 6: Examples & Usability (Week 4-5)

### Example Scripts
- [ ] Create `examples/basic_usage.py` - simplest possible example
- [ ] Create `examples/structured_only.py` - using structured data
- [ ] Create `examples/nlp_only.py` - using only text notes
- [ ] Create `examples/custom_patterns.py` - override default patterns
- [ ] Create `examples/full_pipeline.py` - complete realistic workflow
- [ ] Test all examples work correctly

### Tutorial Notebook
- [ ] Create `examples/tutorial.ipynb`
- [ ] Section 1: Installation and setup
- [ ] Section 2: Basic extraction from text
- [ ] Section 3: Batch processing
- [ ] Section 4: Working with DataFrames
- [ ] Section 5: Validation workflow
- [ ] Include visualizations of results
- [ ] Add explanatory markdown cells

### Error Handling & Logging
- [ ] Add informative error messages throughout
- [ ] Set up logging with `utils/logging_setup.py`
- [ ] Add debug logging for pattern matching
- [ ] Add warning logging for quality flags
- [ ] Make logging configurable (level, output file)

---

## Phase 7: Publishing & CI/CD (Week 5-6)

### GitHub Setup
- [ ] Push all code to GitHub
- [ ] Write detailed `CONTRIBUTING.md`
- [ ] Write `CHANGELOG.md` (start with v0.1.0)
- [ ] Add GitHub issue templates
- [ ] Add pull request template
- [ ] Create badges for README (tests, coverage, PyPI)

### Automated Testing
- [ ] Create `.github/workflows/tests.yml`
- [ ] Set up GitHub Actions to run pytest on push
- [ ] Test on multiple Python versions (3.8, 3.9, 3.10, 3.11)
- [ ] Add code coverage reporting
- [ ] Ensure all tests pass before merging

### Package Publishing
- [ ] Update version to `0.1.0` in `version.py`
- [ ] Test local installation: `pip install -e .`
- [ ] Create account on Test PyPI (test.pypi.org)
- [ ] Publish to Test PyPI: `python -m build && twine upload --repository testpypi dist/*`
- [ ] Test installation from Test PyPI
- [ ] Fix any issues
- [ ] Create account on PyPI (pypi.org)
- [ ] Publish to PyPI: `twine upload dist/*`
- [ ] Test installation: `pip install bc-receptor-extraction`

### Release & DOI
- [ ] Create GitHub release (v0.1.0)
- [ ] Write release notes
- [ ] Link to Zenodo for DOI (makes it citable)
- [ ] Update README with DOI badge
- [ ] Share on Twitter, Reddit r/HealthIT, LinkedIn

---

## Phase 8: Optional Enhancements

### Additional Features
- [ ] Add CLI interface: `bc-receptor extract --input notes.csv --output results.csv`
- [ ] Support for Ki-67 extraction
- [ ] Support for PDL1 extraction
- [ ] ML-based extraction option (BioClinicalBERT)
- [ ] Support for HL7 FHIR format

### Advanced Tools
- [ ] Create web dashboard for validation results
- [ ] Docker container for easy deployment
- [ ] FastAPI service wrapper
- [ ] Integration with popular EHR systems

---

## Success Metrics

### Package Quality
- [ ] 100% of public methods have docstrings
- [ ] >80% test coverage
- [ ] All CI tests passing
- [ ] Installable via pip
- [ ] 3+ working examples

### Clinical Validation
- [ ] Validated on >100 real reports
- [ ] ER accuracy >95%
- [ ] PR accuracy >95%
- [ ] HER2 accuracy >95%
- [ ] Performance metrics documented
- [ ] Error analysis completed

### Community Adoption
- [ ] README with clear badges
- [ ] Contributing guidelines
- [ ] At least 1 external user/tester
- [ ] Published to PyPI
- [ ] DOI for citation

---

## Notes

- Start with Phase 1-2 to get core working quickly
- Validate early and often (Phase 5 is critical!)
- Document as you go, not at the end
- Keep examples simple and realistic
- HER2 2+ logic is the trickiest part - test thoroughly
- Aim for v0.1.0 release after Phase 7

**Current Status:** Phase 1 - Virtual environment created ✓

**Next Steps:** 
1. Initialize git repo
2. Create package structure
3. Start on regex patterns