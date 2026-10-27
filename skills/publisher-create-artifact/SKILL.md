---
name: publisher-create-artifact
description: Create an Oracle Cloud Marketplace Publisher artifact in the OCI Console from a publisher-provided image OCID. Verify the image type, use supplied information, confirm creation, recommend the matching OCI Application Listing package type, and remind the publisher to create a term. Use for publisher artifact onboarding, not consumer image launches or listing publication.
---

# Publisher Artifact Creator

Create an artifact through the Marketplace Publisher UI using information supplied by the publisher. Finish with a listing recommendation and a terms reminder. Invoking this skill for advice or a preview does not authorize creating a live resource.

## Collect missing inputs

Reuse the conversation and authenticated Console context. If the publisher has not supplied an image OCID, ask: “What is the OCID of the image you want to turn into a Marketplace Publisher artifact?” Determine the image type yourself from the resource.

Resolve the intended tenancy/publisher account, region, destination artifact compartment, and artifact name. Ask only for values that cannot be unambiguously resolved. The source image compartment can differ from the artifact compartment. You may propose a name from the verified image name and publisher-provided version; do not invent product or version information. Never request passwords, private keys, or session tokens in chat.

## Identify the image

Retrieve authenticated metadata for the exact OCID and confirm resource kind, name, region, ownership/access, and usable state. An OCID resource-type segment is a routing hint, not proof that the resource exists or is suitable for publication. Do not infer OS, architecture, licensing, or application contents from an OCID string or image name.

| Verified source | Artifact type | Recommended listing |
| --- | --- | --- |
| OCI Compute custom image, commonly `ocid1.image...` | Compute Image (API: `MACHINE_IMAGE`) | OCI Application Listing with Compute Image package |
| OCI Container Registry image, commonly `ocid1.containerimage...` | Container Image (API: `CONTAINER_IMAGE`) | OCI Application Listing with Container Image package |

For Compute images, inspect OS/version, launch options, and compatible shapes when exposed. Report architecture only with authoritative evidence. Confirm eligibility as a custom image; a platform or Marketplace image is not automatically a publishable source.

For container images, inspect the repository, digest/version, registry URL, and source compartment. Match by OCID and digest where available rather than a mutable tag alone. A container image OCID does not establish that a Helm chart exists.

Prefer available read-only OCI tools for metadata; otherwise use the Console's Compute custom image or Container Registry details and pickers. An already configured OCI CLI can provide read-only metadata through `oci compute image get` or `oci artifacts container image get` after checking command help and the selected region. Create the artifact through the UI for this workflow.

If the resource is a stack, chart, boot volume, instance, existing artifact OCID, or another unsupported kind, explain the mismatch and ask for the source image OCID. Do not silently switch types. If metadata cannot be accessed, ask for the correct region/account or required access; do not mistake lack of access for proof that an image does not exist.

## Create in the Publisher Console

Use the available browser/computer-use tool and follow its interaction instructions. Inspect the live UI instead of relying on fixed coordinates, guessed URLs, or hidden browser endpoints. If authenticated UI access is unavailable, preserve the collected inputs and explain the access needed. A filled form is not proof of creation.

1. Navigate to **Marketplace → Publisher → Artifacts**. Check the tenancy, publisher, region, and destination artifact compartment against the request.
2. Look for an existing artifact referencing the same source image in the intended publisher/compartment. Inspect matches; a shared name is insufficient. Reuse an exact match when it satisfies the request and disclose that it already existed. Ask only if multiple matches or a separate requested version make the choice ambiguous.
3. Choose **Create artifact**. Enter the supplied/resolved name, destination compartment, and verified artifact type.
4. Select or enter the exact source image using the live form. For Compute Image, confirm source OCID and region. Supply required username, shape compatibility, or snapshot settings only from publisher instructions or verified metadata. For Container Image, browse the source compartment, select the exact registry image, and verify the resulting registry URL/digest.
5. Ask for required fields whose values remain unknown. Never invent values or select a mandatory guidelines/compliance attestation without supporting evidence or the publisher's confirmation that it is true. If confirmation is needed, prepare the reviewable form first and ask specifically about the attestation.
6. Check the final form against the image OCID, type, name, publisher, region, and destination compartment. If the publisher requested creation, submit **Create artifact** without redundant permission. If they requested preparation/preview only, leave the completed form for review.
7. Open the resulting artifact details. Record the artifact OCID, name, type, source image, compartment, region, Console link, and observed status. Inspect any work request and validation/publication status shown. Distinguish created from ready for a listing: the artifact must be **Available**, with publication **Complete**, to be used for a listing.

Creation can be asynchronous. Check progress with bounded waits and keep the publisher informed. If processing remains underway after about five minutes, return the created artifact link and pending status without implying readiness. On failure, report the actual error/work request and next action. After a timeout or lost response, inspect artifacts/work requests before retrying creation. Stop if the outcome cannot be established instead of risking duplicates.

This artifact-only request does not authorize launching an instance, changing the source image/repository or IAM, creating a listing or term, submitting for review, or publishing. If a separate operation is necessary, explain the dependency and obtain authorization for that operation.

## Return the result and next steps

Give the artifact name, OCID and Console link, verified source image type, and observed readiness. If creation did not succeed, clearly label the result as blocked or prepared and identify the missing input/access.

Recommend **OCI Application Listing** with the package type matching the table. Briefly explain the fit: Compute Image deploys a VM; Container Image distributes a container image package. OS and architecture can refine its description but do not change the listing category. Do not recommend a lead-generation or professional-service listing for a deployable image. Do not infer Free, BYOL, or Paid pricing from metadata; that is the publisher's commercial choice. If OCI Application Listing is absent from the Console, direct the publisher to Oracle Support for account enablement.

Always include this reminder, adapted when a suitable term already exists:

“Remember to create a term and term version with your terms-of-use document in Marketplace Publisher (or reuse an appropriate existing term). You will need to select the terms of use alongside this artifact in the OCI Application Listing's app install package.”

If processing is pending, make the recommendation conditional on the artifact becoming Available. Provide guidance; create a listing or term only when separately requested.

## Authoritative references

Consult these when fields, eligibility, or status behavior need clarification; the live Console can differ by region/account. Do not carry forward legacy Partner Portal region restrictions without current evidence.

- [Create an artifact](https://docs.oracle.com/en-us/iaas/Content/Marketplace/Tasks/create-artifact.htm)
- [Artifact readiness requirements](https://docs.oracle.com/en-us/iaas/Content/Marketplace/Tasks/view-all-artifacts.htm)
- [OCI Application Listing package types and terms selection](https://docs.oracle.com/en-us/iaas/Content/Marketplace/Tasks/creating-oci-application-listing.htm)
- [Manage terms and term versions](https://docs.oracle.com/en-us/iaas/Content/Marketplace/manage-terms.htm)
- [Listing types and account enablement](https://docs.oracle.com/en-us/iaas/Content/Marketplace/manage-listings.htm)
