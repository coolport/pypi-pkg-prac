Full Guide on Resolving PyPI Publish Errors and Versioning in Your Workflow
Error Overview:

The GitHub Action failed when attempting to publish your example_package_coolport-0.0.5-py3-none-any.whl package to PyPI. The reason given in the logs:
Code

Upload failed with status code 400 Bad Request. Server says: 400 File already exists ('example_package_coolport-0.0.5-py3-none-any.whl', with blake2_256 hash ...).

Cause:

    PyPI does not allow overwriting an already uploaded file for any given version.
    The dist/example_package_coolport-0.0.5-py3-none-any.whl file already exists on PyPI, causing the upload to fail.

Steps to Resolve the Error:

    Bump the Version Number:
    To publish a new version, you need to update the version field in your package configuration.

    If you use pyproject.toml:
    TOML

[project]
version = "0.0.6" # Update to the next version

If you use setup.py:
Python

setup(
name="example_package_coolport",
version="0.0.6", # Update the version here
...
)

Commit this change:
sh

git add pyproject.toml
git commit -m "Bumped version to 0.0.6"

Trigger the Workflow:

You can push the commit directly or create a tag for the new version, depending on your workflow setup.

Using a Commit: Push your branch to trigger the workflow:
sh

git push origin <branch>

Using a Tag:
If your workflow triggers on tags, create a new tag for the updated version:
sh

    git tag v0.0.6
    git push origin v0.0.6

Versioning Best Practices (SemVer Guide):

Semantic Versioning (SemVer) specifies how to format package versions. Versions follow the pattern:

<MAJOR>.<MINOR>.<PATCH>

    Patch (0.0.X):
    Increment for bug fixes or small changes. Example: 0.0.5 → 0.0.6.

    Minor (<X>.X.0):
    Increment for backward-compatible additions or features. Example: 0.1.0 → 0.2.0.

    Major (X.X.X):
    Increment for breaking changes or incompatible updates. Example: 1.0.0 → 2.0.0.

    Pre-release Versions (-alpha, -beta, -rc):
    Append a suffix for pre-release or testing builds. Examples:
        0.1.0-alpha (Unstable, experimental release).
        0.1.0-beta (Closer to stable).
        1.0.0-rc.1 (Release candidate; nearly stable).

    Build Metadata (+...): Non-ordering metadata can be appended to clarify build context: Example: 1.0.0-alpha+build.1848 for internal versions.

Considerations for Tags:

You mentioned the possibility of using tags like v0.0.91 or appending custom messages (e.g., v0.0.9-something).

    Is 0.0.91 valid? Yes, v0.0.91 is a valid SemVer version. While you're in pre-1.0 development, you can make as many increments as needed (e.g., 0.0.5 → 0.0.91).

    Custom Tags (X.X.X-MESSAGE) in Convention:
        Suffixes (-MESSAGE) are typically reserved for pre-releases like -alpha, -hotfix, or -rc.1.
        Avoid creative or non-standard suffixes for production tags, as they may confuse others.

Workflow Re-Triggering Scenarios:

    Push New Tags (Clean Approach):
        Best practice is to tag every version update (v0.0.6, v0.0.7, etc.).
        Each tag points to a commit with a version bump.

    Re-Tagging Same Version (Not Recommended):
        If you need v0.0.5 to include the new change:
            Delete the tag locally and remotely:
            sh

git tag -d v0.0.5
git push origin :v0.0.5

Re-tag and push:
sh

        git tag v0.0.5
        git push origin v0.0.5

    Caution: This rewrites history and can cause confusion in teams.

Trigger Without a Tag:

    If your workflow allows, push directly to main or a release branch:
    sh

        git push origin main

General Workflow for Updates:

    Make code or configuration changes.
    Increment the version (pyproject.toml or setup.py).
    Commit and push the changes:
    sh

git commit -am "Fix configuration and bump version to 0.0.6"
git push origin main

Create and push a new tag for the release:
sh

    git tag v0.0.6
    git push origin v0.0.6

Your publish workflow should take care of releasing the package to PyPI or other repositories from there.
Summary of Key Points:

    The failure occurred because PyPI does not allow re-uploading files with the same version and hash.
    Solution: Bump your package version in pyproject.toml/setup.py and re-trigger the workflow.
    Follow Semantic Versioning (MAJOR.MINOR.PATCH) to version the package consistently.
    Use tags to delineate specific versions, and avoid overwriting tags to keep history intact.
    Consider standard pre-release suffixes if needed (-beta, -rc.1, etc.).

By following these guidelines, you'll have a robust process for releasing, versioning, and managing your software updates
