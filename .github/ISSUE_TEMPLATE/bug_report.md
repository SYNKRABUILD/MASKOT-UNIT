name: Joke Generator Bug Report
description: Report a bug in the joke generator feature
title: "[BUG] Joke Generator - "
labels: ["bug", "joke-generator"]
assignees: []

body:
  - type: markdown
    attributes:
      value: |
        Thanks for reporting a bug! Please fill in the details below.
  
  - type: textarea
    id: description
    attributes:
      label: Description
      description: Clear description of the bug
      placeholder: "Describe what went wrong..."
    validations:
      required: true
  
  - type: textarea
    id: steps
    attributes:
      label: Steps to Reproduce
      description: How to reproduce the issue
      placeholder: |
        1. Click on...
        2. Then...
        3. The bug appears...
    validations:
      required: true
  
  - type: textarea
    id: expected
    attributes:
      label: Expected Behavior
      description: What should have happened
      placeholder: "The joke should load and display..."
    validations:
      required: true
  
  - type: textarea
    id: actual
    attributes:
      label: Actual Behavior
      description: What actually happened
      placeholder: "The page shows an error..."
    validations:
      required: true
  
  - type: input
    id: environment
    attributes:
      label: Environment
      description: Browser, OS, or other relevant details
      placeholder: "Chrome 120, Windows 11"
    validations:
      required: false
  
  - type: textarea
    id: logs
    attributes:
      label: Error Logs
      description: Any error messages or console logs
      placeholder: "Paste error logs here..."
    validations:
      required: false
