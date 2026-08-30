name: Feature Request
description: Suggest a new feature or enhancement
title: "[FEATURE] "
labels: ["enhancement"]
assignees: []

body:
  - type: markdown
    attributes:
      value: |
        Share your feature idea! Help us improve Maskot Unit.
  
  - type: textarea
    id: description
    attributes:
      label: Feature Description
      description: Clear description of the feature
      placeholder: "I'd like to add..."
    validations:
      required: true
  
  - type: textarea
    id: motivation
    attributes:
      label: Motivation
      description: Why is this feature needed? How will it help creators?
      placeholder: "This feature would help because..."
    validations:
      required: true
  
  - type: textarea
    id: use-case
    attributes:
      label: Use Case
      description: Real-world example or scenario
      placeholder: "A creator could use this to..."
    validations:
      required: true
  
  - type: textarea
    id: implementation
    attributes:
      label: Proposed Implementation
      description: How you think this could be implemented
      placeholder: "This could be done by..."
    validations:
      required: false
  
  - type: checkboxes
    id: terms
    attributes:
      label: Community Standards
      options:
        - label: I've searched for similar feature requests
          required: true
        - label: This feature aligns with Maskot Unit's mission
          required: true
