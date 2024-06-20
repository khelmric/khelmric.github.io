# Using the Organization Policy API in Python

**Tags:** GCP, Python

**Creation Date:** 2023-11-17

## Description

The Python Client for the Organization Policy API provides access to query the policies on Organization, Folder or Project level. 
The Python iterates over all policies on the selected resource and provides the data in JSON format.

## Prerequisites

- The Organization Policy API (orgpolicy.googleapis.com) has to be enabled in the project, where the Python script will be executed.
- The executor needs the Organization Policy Viewer role (roles/orgpolicy.policyViewer) on the selected resource.
- The Google Cloud Organization Policy API client library (google-cloud-org-policy) is required for the Python script (e.g. google-cloud-org-policy==1.8.3 in the requirements.txt file).

## Solution

```python
# Python code example
import google.cloud.orgpolicy