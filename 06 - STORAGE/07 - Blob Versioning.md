# Azure Storage - Blob Versioning

Blob versioning is a feature in Azure Storage that automatically maintains previous versions of an object. When enabled, Azure Storage creates a new version of a blob each time it is modified or deleted, allowing you to easily restore earlier states of your data.

## Key Benefits
* **Data Protection:** Protects against accidental deletion or modification by preserving the history of a blob.
* **Point-in-Time Recovery:** Easily retrieve and restore previous versions of a blob if the current version is corrupted or overwritten.
* **Seamless Integration:** Works automatically in the background once enabled at the storage account level.

## How It Works
* **Modifications:** Overwriting an existing blob automatically moves the current state to a previous version, and the new upload becomes the current version.
* **Deletions:** Deleting a blob does not remove it permanently; instead, the current version becomes a previous version, and the blob is no longer visible unless versions are explicitly requested.
* **Version IDs:** Each version is uniquely identified by a version ID based on the timestamp of when it was created.

## Video Tutorial

Click the image below to watch the full tutorial on Azure Storage Blob Versioning:

[![Azure Storage Blob Versioning Tutorial](https://img.youtube.com/vi/u0gaWy4RBv4/maxresdefault.jpg)](https://youtu.be/u0gaWy4RBv4)