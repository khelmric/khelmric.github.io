# Using the Organization Policy API in Python

**Tags:** GCP, Python

**Creation Date:** 2023-11-17

**Author:** Kristof Helmrich

## Description

The Python Client for the Organization Policy API provides access to query the policies on Organization, Folder or Project level. 
The Python iterates over all policies on the selected resource and provides the data in JSON format.

## Prerequisites

- The Organization Policy API (orgpolicy.googleapis.com) has to be enabled in the project, where the Python script will be executed.
- The executor needs the Organization Policy Viewer role (roles/orgpolicy.policyViewer) on the selected resource.
- The Google Cloud Organization Policy API client library (google-cloud-org-policy) is required for the Python script (e.g. google-cloud-org-policy==1.8.3 in the requirements.txt file).

## Solution

### Get the list of Organization Policies

```python
from google.cloud import orgpolicy_v2
# Set the Org ID
resource = "organizations/00000000"
# Create client
orgpol_client = orgpolicy_v2.OrgPolicyClient()
# Initialize request argument(s)
orgpol_request = orgpolicy_v2.ListPoliciesRequest(
    parent=resource
)
# Make the request
orgpol_page_results = orgpol_client.list_policies(request=orgpol_request)
# Iterate over the results
for item in orgpol_page_results:
    print(item)
```

#### Output

```json
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: *********
  }
  rules {
    values {
      allowed_values: "C1**********"
      allowed_values: "C2**********"
      allowed_values: "C3**********"
      allowed_values: "C4**********"
      allowed_values: "C5**********"
    }
  }
}

name: "organizations/************/policies/gcp.resourceLocations"
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: *********
  }
  rules {
    values {
      allowed_values: "in:europe-locations"
    }
  }
}

name: "organizations/************/policies/storage.uniformBucketLevelAccess"
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: *********
  }
  rules {
    enforce: true
  }
}

name: "organizations/************/policies/compute.requireOsLogin"
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: ********
  }
  reset: true
}

name: "organizations/************/policies/compute.skipDefaultNetworkCreation"
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: *********
  }
  rules {
    enforce: true
  }
}

name: "organizations/************/policies/compute.vmExternalIpAccess"
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: *********
  }
  rules {
    allow_all: true
  }
}

name: "organizations/************/policies/iam.automaticIamGrantsForDefaultServiceAccounts"
spec {
  etag: "**********"
  update_time {
    seconds: **********
    nanos: ********
  }
  rules {
    enforce: true
  }
}
```

## Notes
The Organization Policies can be listed on Organization (resource = "organizations/00000000"), on Folders (resource = "folders/00000000") or on Project resources (resource = "projects/00000000").

## References
- https://cloud.google.com/resource-manager/docs/organization-policy/overview
- https://cloud.google.com/python/docs/reference/orgpolicy/latest
- https://pypi.org/project/google-cloud-org-policy/