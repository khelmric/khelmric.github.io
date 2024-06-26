# Using the Security Command Center API in Python

**Tags:** GCP, Python

**Creation Date:** 2023-11-17

**Author:** Kristof Helmrich

## Description

The Security Command Center (SCC) in Google Cloud has a description about almost all resources within the Organization and about their configuration. The Python SCC query iterates over all selected resources (according to the defined filter) and provides the data in JSON format.

## Prerequisites

- The Security Command Center API (securitycenter.googleapis.com) has to be enabled in the project, where the Python script will be executed
- The executor needs the Security Center Admin Viewer role (roles/securitycenter.adminViewer) on the selected Organization
- The Google Cloud Securitycenter API client library (google-cloud-securitycenter) is required for the Python script (e.g. google-cloud-securitycenter==1.23.3  in the requirements.txt file)

## Solution

### Get information about organisation, folders and projects

```python
from google.cloud import securitycenter_v1

# Set the Org ID
org_resource = "organizations/0000000000"

# Create client
scc_client = securitycenter_v1.SecurityCenterClient()

# Create a filter for specific resources
scc_search_filter = "securityCenterProperties.resource_type=\"google.cloud.resourcemanager.Organization\" OR securityCenterProperties.resource_type=\"google.cloud.resourcemanager.Folder\" OR securityCenterProperties.resource_type=\"google.cloud.resourcemanager.Project\""

# Initialize request argument(s)
scc_request = securitycenter_v1.ListAssetsRequest(
    parent=org_resource,
    filter=scc_search_filter,
)

# Make the request
scc_page_results = scc_client.list_assets(request=scc_request)

# Iterate over the results
for item in scc_page_results:
    print(item)
```

#### Output

```json
asset {
  name: "organizations/************/assets/*******************"
  security_center_properties {
    resource_name: "//cloudresourcemanager.googleapis.com/organizations/************"
    resource_type: "google.cloud.resourcemanager.Organization"
    resource_display_name: "gcp.***.com"
  }
  resource_properties {
    key: "creationTime"
    value {
      string_value: "****-**-**T**:**:**.***Z"
    }
  }
  resource_properties {
    key: "displayName"
    value {
      string_value: "gcp.***.com"
    }
  }
  resource_properties {
    key: "lifecycleState"
    value {
      string_value: "ACTIVE"
    }
  }
  resource_properties {
    key: "name"
    value {
      string_value: "organizations/************"
    }
  }
  resource_properties {
    key: "organizationId"
    value {
      string_value: "************"
    }
  }
  resource_properties {
    key: "owner"
    value {
      string_value: "{\"directoryCustomerId\":\"C********\"}"
    }
  }
  security_marks {
    name: "organizations/************/assets/*******************/securityMarks"
  }
  create_time {
    seconds: **********
    nanos: *********
  }
  update_time {
    seconds: **********
    nanos: *********
  }
  iam_policy {
    policy_blob: "{\"bindings\":[{\"role\":\"organizations/************/roles/CustomRole***\",\"members\":[\:***@***.***\"]},{\"role\":\"organizations/************/roles/PocCostControlCustomRole\",\"members\":[\:***@***.***\"]},{\"role\":\"roles/assuredworkloads.admin\",\"members\":[\:***@***.***\"]}],\"auditConfigs\":[{\"service\":\"allServices\",\"auditLogConfigs\":[{\"logType\":\"DATA_WRITE\"},{\"logType\":\"DATA_READ\"},{\"logType\":\"ADMIN_READ\"}]},{\"service\":\"cloudbilling.googleapis.com\",\"auditLogConfigs\":[{\"logType\":\"ADMIN_READ\"},{\"logType\":\"DATA_READ\"},{\"logType\":\"DATA_WRITE\"}]},{\"service\":\"cloudresourcemanager.googleapis.com\",\"auditLogConfigs\":[{\"logType\":\"ADMIN_READ\"},{\"logType\":\"DATA_READ\"},{\"logType\":\"DATA_WRITE\"}]}]}"
  }
  canonical_name: "organizations/************/assets/*******************"
}
...
asset {
  name: "organizations/************/assets/*******************"
  security_center_properties {
    resource_name: "//cloudresourcemanager.googleapis.com/folders/************"
    resource_type: "google.cloud.resourcemanager.Folder"
    resource_parent: "//cloudresourcemanager.googleapis.com/folders/************"
    resource_display_name: "*********"
    resource_parent_display_name: "*********"
    folders {
      resource_folder: "//cloudresourcemanager.googleapis.com/folders/************"
      resource_folder_display_name: "*********"
    }
    folders {
      resource_folder: "//cloudresourcemanager.googleapis.com/folders/************"
      resource_folder_display_name: "*********"
    }
  }
  resource_properties {
    key: "createTime"
    value {
      string_value: "****-**-**T**:**:**.***Z"
    }
  }
  resource_properties {
    key: "displayName"
    value {
      string_value: "*********"
    }
  }
  resource_properties {
    key: "lifecycleState"
    value {
      string_value: "ACTIVE"
    }
  }
  resource_properties {
    key: "name"
    value {
      string_value: "folders/************"
    }
  }
  resource_properties {
    key: "parent"
    value {
      string_value: "folders/************"
    }
  }
  security_marks {
    name: "organizations/************/assets/*******************/securityMarks"
  }
  create_time {
    seconds: **********
    nanos: *********
  }
  update_time {
    seconds: **********
    nanos: *********
  }
  iam_policy {
    policy_blob: "{\"bindings\":[{\"role\":\"roles/resourcemanager.folderAdmin\",\"members\":[\:***@***.***\",\:***@***.***\"]},{\"role\":\"roles/resourcemanager.folderEditor\",\"members\":[\:***@***.***\"]},{\"role\":\"roles/resourcemanager.folderIamAdmin\",\"members\":[\:***@***.***\"]},{\"role\":\"roles/resourcemanager.projectIamAdmin\",\"members\":[\:***@***.***\"]},{\"role\":\"roles/viewer\",\"members\":[\:***@***.***\"]}]}"
  }
  canonical_name: "folders/************/assets/*******************"
}
...
asset {
  name: "organizations/************/assets/*******************"
  security_center_properties {
    resource_name: "//cloudresourcemanager.googleapis.com/projects/************"
    resource_type: "google.cloud.resourcemanager.Project"
    resource_parent: "//cloudresourcemanager.googleapis.com/folders/************"
    resource_project: "//cloudresourcemanager.googleapis.com/projects/************"
    resource_owners: :***@***.***"
    resource_display_name: "*********"
    resource_parent_display_name: "*********"
    resource_project_display_name: "*********"
    folders {
      resource_folder: "//cloudresourcemanager.googleapis.com/folders/************"
      resource_folder_display_name: "*********"
    }
    folders {
      resource_folder: "//cloudresourcemanager.googleapis.com/folders/************"
      resource_folder_display_name: "*********"
    }
  }
  resource_properties {
    key: "createTime"
    value {
      string_value: "****-**-**T**:**:**.***Z"
    }
  }
  resource_properties {
    key: "lifecycleState"
    value {
      string_value: "ACTIVE"
    }
  }
  resource_properties {
    key: "name"
    value {
      string_value: "*********"
    }
  }
  resource_properties {
    key: "parent"
    value {
      string_value: "{\"id\":\"************\",\"type\":\"folder\"}"
    }
  }
  resource_properties {
    key: "projectId"
    value {
      string_value: "*********"
    }
  }
  resource_properties {
    key: "projectNumber"
    value {
      string_value: "************"
    }
  }
  security_marks {
    name: "organizations/************/assets/*******************/securityMarks"
  }
  create_time {
    seconds: **********
    nanos: *********
  }
  update_time {
    seconds: **********
    nanos: *********
  }
  iam_policy {
    policy_blob: "{\"bindings\":[{\"role\":\"roles/artifactregistry.serviceAgent\",\"members\":[\:***@***.***\"]},{\"role\":\"roles/cloudbuild.builds.builder\",\"members\":[\:***@***.***\"]},{\"role\":\"roles/cloudbuild.serviceAgent\",\"members\":[\:***@***.***\"]}]}"
  }
  canonical_name: "projects/************/assets/*******************"
} 
```

### List all resource types in the Organization 

```python
from google.cloud import securitycenter_v1

# Set the Org ID
org_resource = "organizations/0000000000"

# Create client
scc_client = securitycenter_v1.SecurityCenterClient()

# Create a filter for specific resources

# Initialize request argument(s)
scc_request = securitycenter_v1.ListAssetsRequest(
    parent=org_resource,
)

# Make the request
scc_page_results = scc_client.list_assets(request=scc_request)

# Create an empty list for the resource types
scc_resource_types = []

# Iterate over the results
for item in scc_page_results:
    scc_resource_types.append(item.asset.security_center_properties.resource_type)

for uniq_item in (sorted(set(scc_resource_types))):
    print(uniq_item)
```

#### Output

```json
google.appengine.Application
google.appengine.Service
google.appengine.Version
google.cloud.bigquery.Dataset
google.cloud.dns.ManagedZone
google.cloud.kms.CryptoKey
google.cloud.kms.CryptoKeyVersion
google.cloud.kms.ImportJob
google.cloud.kms.KeyRing
google.cloud.resourcemanager.Folder
google.cloud.resourcemanager.Organization
google.cloud.resourcemanager.Project
google.cloud.sql.Instance
google.cloud.storage.Bucket
google.cloudfunctions.CloudFunction
google.compute.Address
google.compute.Disk
google.compute.Firewall
google.compute.Image
google.compute.Instance
google.compute.InstanceGroup
google.compute.Network
google.compute.Project
google.compute.ResourcePolicy
google.compute.Route
google.compute.Router
google.compute.Snapshot
google.compute.SslCertificate
google.compute.Subnetwork
google.containerregistry.Image
google.gkehub.Membership
google.iam.Role
google.iam.ServiceAccount
google.iam.ServiceAccountKey
google.logging.LogBucket
google.logging.LogMetric
google.logging.LogSink
google.networkmanagement.ConnectivityTest
google.pubsub.Subscription
google.pubsub.Topic
google.run.Revision
google.run.Service
google.secretmanager.Secret
google.secretmanager.SecretVersion
google.serviceusage.Service
```

## Notes
- Filtering the resources is important, otherwise the query will take a long time to get the results, especially in case of huge Organizations with many resources 
- Organization Policies are not included to the Security Command Center resources. To query Org Policies use the  google-cloud-org-policy  Python client library.

## References
- https://cloud.google.com/security-command-center?hl=en
- https://cloud.google.com/python/docs/reference/securitycenter/latest
- https://pypi.org/project/google-cloud-securitycenter/