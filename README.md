# Seerr – Email Template Refactor & i18n (Fork)

This fork is based on the official Seerr repository and refactors the existing email system from partially hardcoded text to a fully template-based structure.

As part of this change, all English email content has been moved from the codebase into Pug templates. These English templates preserve the original behavior and serve as the reference implementation.

With this approach, additional languages can now be added in a structured way using subfolders (e.g. `de/`), without requiring any changes to the application logic.

The goal of this refactor is a clear separation between logic and presentation, as well as a sustainable foundation for multilingual support and future extensions.

---

## Architecture & Functionality

Email template selection is performed dynamically at runtime based on configured language settings.

### Template Selection (Locale Chain)

For each email, the appropriate template is selected using the following order:

1. User language (`User.settings.locale`)
2. Global system language (`Settings.main.locale`)
3. Fallback to English (default templates)

Selection is based on the existence of template files. If a template exists for a given language, it will be used; otherwise, the system automatically falls back to the next level.

---

### Directory Structure

Templates are organized as follows:

```
server/templates/email/
├── media-request/
├── media-issue/
├── resetpassword/
├── generatedpassword/
├── test-email/
└── de/
    ├── media-request/
    ├── media-issue/
    ├── resetpassword/
    ├── generatedpassword/
    └── test-email/
```

- The root structure represents the English templates (reference)
- Language-specific templates are placed in subfolders (`de/`, others in future)
- Each language mirrors the same structure as the English templates

---

### Template Structure

A template typically consists of multiple files with clear responsibilities:

- `html.pug`  
  Main template for the email content (layout + rendering)

- `subject.pug`  
  Email subject line

- `body.pug` *(optional)*  
  Additional structure for complex content or reuse within templates

- `_helpers.pug`  
  Shared helper functions and language logic (e.g. grammar, labels, formatting)

---

### Architectural Goals

This structure enables:

- clear separation of logic and presentation
- easy extensibility for additional languages
- consistent behavior across all templates
- full control over email content without modifying application code

---

## Extensions & Features

In addition to the template refactor, additional functionality has been implemented to support proper language-dependent rendering.

### Language-Specific Text Logic

Reusable helper functions are provided via `_helpers.pug`, encapsulating language-specific logic.

This includes:

- correct grammatical phrasing (e.g. “the movie” vs. “the series”)
- language-specific terminology
- centralized handling of recurring text elements

All of this logic resides entirely within the templates, not in application code.

---

### Dynamic Label Handling

Certain metadata and labels are adjusted depending on the language.

Examples:

- “Requested Seasons” → “Angefragte Staffel” / “Angefragte Staffeln”
- automatic singular/plural handling
- consistent naming across all templates

---

### Consistent Template Structure

All templates, regardless of language, strictly follow the structure of the English reference templates.

This ensures:

- identical behavior across all languages
- easier maintenance
- straightforward comparison with future upstream changes

---

### Extensibility

New languages can be added without modifying application code:

1. Create a new language directory (e.g. `fr/`, `it/`)
2. Copy the structure of the English templates
3. Translate the content

All language logic remains fully contained within the templates.

---

## Languages & Roadmap

### Currently Supported Languages

- English (reference)
- German

English serves as the base for all templates and reflects the original Seerr behavior.

---

### Planned Extensions

The template structure allows additional languages to be added at any time.

Possible future languages include:

- French
- Spanish
- Italian
- others as needed

---

## Note

This fork only extends the email system of Seerr.

For installation, configuration, and all other features, please refer to the official repository:

https://github.com/seerr-team/seerr
