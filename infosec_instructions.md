# Infosec GHSA Process Proposal

Roles & Responsibilities:

* *Reporter*
  * Responsibility:
    1. Submits a `potential vulnerability` to the Infosec group
* *Remediation Coordinator*
  * Responsibility:
    1. Coordinates the community
        * Finds a *remediation developer*
        * Finds 2x *Remediation Reviewers*
    2. Acts as the one voice for responding to the *Reporter*
    3. Should be a [Tianocore Owner](https://github.com/orgs/tianocore/people?query=role%3Aowner) or coordinate with one. They are the only ones who can create a new GHSA
* *Remediation Developer*
  * Responsibility:
    1. Patching the vulnerability
    2. Testing the patch
        * Must write up what tests they've ran and include information about Platforms / Conditions / Environment
* *Remediation Reviewer*
  * Responsibility:
    1. *Subject matter experts* (SMEs) that should be available to answer questions posed by the *remediation developer*
    2. Testing the patch
      * Must write up what tests they've ran and include information about Platforms / Conditions / Environment
* *Package Maintainer*
  * Responsibility:
    1. The final shepard of the patch from the advisory branch to main

## Stages

The following are the stages that a vulnerability goes through until being patched.

### Stage: Vulnerability Report

A Security Advisory begins when a **reporter** reports a vulnerability.

A `Reporter` will hit the `Security` tab on [Edk2](https://github.com/tianocore/edk2/security) to begin.

![Report a Vulnerability](images\report_a_vulnerability.png)

Now the `Reporter` can begin submitting vulnerability details.
![Vulnerability Details](images\submit_vulnerability_details.png)

At this point the `Reporter` is waiting for response from the EDK2 Info Sec group.

Alternatively a report may be given by VINCE. In that case, the reporter should be directed
to the github security page to submit the security issue for internal tracking.

### Stage: Submission Review

When a new CVE is identified, a `Remediation Coordinator` must be designated (usually at an Infosec meeting).

The `Remediation Coordinator` must then find the following:

1. `Remediation Developer`
2. 2x `Remediation Reviewers` (ideally SMEs of the patch in the Infosec community)

Once these roles are filled, the process can move on to the patching stage.

If the submission is not found to be a CVE, the `Remediation Coordinator` must work with the submitter to close the issue. The `Reporter` may write up public documentation.

### Stage: Patch

During this stage, the `Remediation Coordinator` will inform the `Reporter` that the 180 (90?) day embargo has begun. The `Remediation Coordinator` should provide the `Reporter` with up-to-date information and be the **ONLY** voice discussing when patches will be available.

The `Remediation Coordinator` should then create a new GHSA and a temporary fork for the Advisory. Each Advisory will have exactly one fork that may contain multiple branches, forked from EDK2.

![create temporary fork](images\create_temporary_fork.png)

The `Remediation Developer` will perform their work and create a patch as soon as possible. If they have questions
they should contact the `Remediation Reviewers` ASAP.

The `Remediation Developer` must update the appropriate package security vulnerabilities json to indicate what is being patched.

### Stage: Review

When the `Remediation Developer` is ready for a review, they will create a review on the private fork targeting the main branch. (NOTE: this review will be abandoned).

![review](images\targetable_branch.jpg)

The pipelines are not expected to work. To reduce the work that will need to take place in the public, the `Remediation Developer` should run as many pipelines locally as they can (formatting, etc).

(TODO: can we script this / formalize this?)

During this stage, the `Remediation Reviewers` must be responsive. If one or both reviewers are unresponsive, the `Remediation Coordinator` should contact them as soon as possible. If unresponsiveness continues, the `Remediation Coordinator` should involve a maintainer to push the patch forward quickly.

The `Remediation Coordinator` should encourage the `Reporter` to review the patch and verify if they are able to reproduce the original vulnerability.

The review will be considered complete when both `Remediation Reviewers` have accepted the patch.

### Stage: Publish

Once the review is complete, the `Remediation Coordinator` must work with the edk2 maintainers (or delegate branch creation to infosec via some method, such as a bot) to create a branch that follows the format "/security/advisory/cve-xxx-xxxx".

This special branch will not be the final destination but simply a means to publish the branch so that the community can be advised and comment. However this branch must be merged in within 24 hours.

Next, the `Remediation Developer` should target the new branch, and the `Remediation Reviewers` should approve the new review.

The `Remediation Coordinator` may then publish the GHSA and credit the appropriate parties.

![publish advisory](images\publish_advisory.png)

### Stage: Merge into Main

The final stage is to merge the patch into main. Here the *Remediation Coordinator* will work with the *Package Maintainer* to create a pull request against main. The community will have one final ability to comment but merging should be the priority over nitpicking.
Once merged the process is complete.
