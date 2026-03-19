---
description: How does this system work?
icon: trowel-bricks
metaLinks:
  alternates:
    - https://app.gitbook.com/s/ocz2kFW2PRMWNpWEjhbc/build-system/structure
---

# Structure

Our build and pack system follows the structure that is outlined below to give you the ability to create a powerful build system for your projects to ensure that you provide developers short commands to build the project and to simplify the complicated build operations.

***

## <mark style="color:$primary;">Required structure</mark>

The following structure is required for every project that uses this build toolset:

* Project directory
  * `tools` (directory, cloned as a git submodule. Consult the source code for more info.)
  * `vendor` (directory, you'll have to create the below files and this directory yourself)
    * `vnd_build.py` (file, list of instructions for build)
    * `vnd_clean.py` (file, list of instructions for cleanup)
    * `vnd_test.py` (file, list of instructions for test)
    * `vnd_increment.py` (file, list of instructions for version incrementation)
    * `vnd_vendorize.py` (file, list of instructions for dependency vendoring)
    * `vnd_gendocs.py` (file, list of instructions for documentation generation)
    * `vnd_packdocs.py` (file, list of instructions  for documentation packing)
    * `vnd_packbin.py` (file, list of instructions for artifact packing)
    * `vnd_pushbin.py`  (file, list of instructions for pushing to package registry)
    * `vnd_liquidize.py`  (file, list of instructions for project obsoletion)
    * `vnd_updatedeps.py`  (file, list of instructions for updating project dependencies)
    * `vnd_listprojs.py`  (file, list of instructions for listing project files)

{% hint style="info" %}
You don't have to create every vendor script file. You'll only have to create the files as per your project's requirements.
{% endhint %}

{% hint style="info" %}
Dependency vendoring may be required for offline builds, especially .NET projects that use NuGet to fetch their dependencies. In this case, you'll have to implement the `localize` function.
{% endhint %}

How the project calls the build scripts is entirely up to the project and not to a standard Makefile that makes use of those scripts found in the tools directory.

***

## <mark style="color:$primary;">Vendor Python scripts</mark>

When you begin writing vendor scripts for your projects, you'll have to consider the following:

<details>

<summary>Argument signatures for some actions</summary>

Not all actions that ADT supports contain the same argument signatures. Therefore, it's best to follow this:

*   Build arguments and test arguments are provided with the first argument, `build_args` and `test_args`, before the extra arguments, `extra_args`. Therefore, you'll need to provide those two arguments when defining the `vnd_build()` and the `vnd_test()` functions, like this:

    ```python
    def vnd_build(args, extra_args):
        # Your code here
    ```
* Extra arguments are arguments that are unknown to the ADT hook for each action. However, `build_args` is populated by `-b` (`--build-args`) switch, and `test_args` is populated by `-t` (`--test-args`) switch.
*   Increment action doesn't support extra arguments, because it assumes that, after the old version (`old_version`) and the new version (`new_version`) is specified, extra arguments are specified as extra versions (`api_versions`), such as codenames or API versions. However, you'll need to define all three arguments when defining `vnd_increment()`, like this:

    ```python
    def vnd_increment(old_version, new_version, api_versions):
        # Your code here
    ```

</details>

<details>

<summary>Pre-action and post-action events</summary>

If you want to specify the instructions that you want to run before the real event, you'll have to define either one or both of the parameterless functions that are named like this:

* `vnd_preaction()`: Contains instructions that are executed before the action
* `vnd_postaction()`: Contains instructions that are executed after the successful action run.

For example, `vnd_prebuild()` defines pre-build actions and `vnd_postbuild()` defines post-build actions, and are located in the `vnd_build.py` module.

{% hint style="info" %}
Please note that the arguments are not supported in these functions.
{% endhint %}

</details>

***

## <mark style="color:$primary;">Other tools</mark>

When it comes to ADT's other tools, here are the available tools that you can use:

<details>

<summary>Standalone actions</summary>

<table><thead><tr><th width="120">Action</th><th>Usage</th><th>Description</th></tr></thead><tbody><tr><td><code>intreport</code></td><td><code>-l [-lp path]</code></td><td>Generates a report to a local directory</td></tr><tr><td></td><td><code>-r [-rp path] [-sh host] [-sp port] [-su username] [-sp password]</code></td><td>Generates a report to a remote directory over SSH with password</td></tr><tr><td></td><td><code>-r [-rp path] [-sh host] [-sp port] [-su username] [-sk privkeypath]</code></td><td>Generates a report to a remote directory over SSH with private key</td></tr><tr><td><code>dnresxlang</code></td><td><code>json_path -ac -l language -c culture [-c culture2] [-c culture3]...</code></td><td>Adds cultures to an existing language</td></tr><tr><td></td><td><code>json_path -ai -l language -is locid locstr [-is locid2 locstr2] [-is locid3 locstr3]...</code></td><td>Adds localizations and their strings to an existing language</td></tr><tr><td></td><td><code>json_path -al -l language -c culture [-c culture2] [-c culture3]...</code></td><td>Adds a language with specified cultures</td></tr><tr><td></td><td><code>json_path -dc -l language -c culture [-c culture2] [-c culture3]...</code></td><td>Deletes cultures from a specific language</td></tr><tr><td></td><td><code>json_path -di -l language -i locid[-i locid2] [-i locid3]...</code></td><td>Deletes localizations from a specific language</td></tr><tr><td></td><td><code>json_path -dl -l language</code></td><td>Deletes a language</td></tr><tr><td></td><td><code>json_path -ec -l language -c culture [-c culture2] [-c culture3]...</code></td><td>Replaces a list of cultures with specified cultures</td></tr><tr><td></td><td><code>json_path -ei -l language -is locid locstr [-is locid2 locstr2] [-is locid3 locstr3]...</code></td><td>Edits existing localizations for new strings</td></tr><tr><td></td><td><code>json_path -r [-l language]</code></td><td>Generates a report for a specific language or for all languages</td></tr><tr><td></td><td><code>json_path -s -rp resx_path</code></td><td>Saves all languages and converts them for use with .NET projects</td></tr></tbody></table>

{% hint style="warning" %}
The `dnresxlang` action only supports .NET projects. You can find the JSON structure of a language file [here](https://app.gitbook.com/s/bKrTzoXx2e0avHOvf94R/usage/localization-generation).
{% endhint %}

</details>

<details>

<summary>Git-specific actions</summary>

<table><thead><tr><th width="109.33331298828125">Action</th><th>Usage</th><th>Description</th></tr></thead><tbody><tr><td><code>tags</code></td><td></td><td>Lists tags for the whole repo</td></tr><tr><td><code>branches</code></td><td></td><td>Lists checked-out branches</td></tr><tr><td><code>commits</code></td><td></td><td>Lists commits in the current branch</td></tr><tr><td><code>status</code></td><td></td><td>Lists status of the local repo</td></tr><tr><td><code>revert</code></td><td><code>commit</code></td><td>Creates a commit that reverts a commit</td></tr><tr><td><code>commit</code></td><td><code>-s summary -t type [-b body] [-a attributes] [-i --assistant model] [-c commit]</code></td><td>Creates a conventional commit with summary and <a href="https://app.gitbook.com/s/Id4bob6wnHvpX4zbVVtI/guidelines/contribution-guidelines#contribution-guidelines">type</a></td></tr><tr><td><code>push</code></td><td><code>[-r remote]</code></td><td>Pushes commits to either the current remote or a specific remote</td></tr><tr><td><code>reset</code></td><td><code>commit</code></td><td>Resets to a commit</td></tr><tr><td><code>hardclean</code></td><td></td><td>Forces destructive removal of untracked files, even ignored ones (executes <code>git clean -xdf</code>)</td></tr></tbody></table>

{% hint style="warning" %}
In order to be able to use Git-specific actions, you'll have to clone ADT as submodule in your repository after initializing it. You may need to use the `git` command directly in some situations.
{% endhint %}

</details>

{% hint style="info" %}
You can pass `-v` to any ADT command to enable verbose output, and you can pass `--nobanner` to prevent showing version information.
{% endhint %}

{% hint style="warning" %}
To be able to use other tools, you'll have to either install dependencies from your system package manager outlined in the [`requirements.txt`](https://github.com/Aptivi/tools/blob/main/requirements.txt) file, or you'll have to use `pip install -r tools/requirements.txt` to install the required dependencies. You can learn more about requirements.txt [here](https://packaging.python.org/en/latest/tutorials/installing-packages/#requirements-files).
{% endhint %}
