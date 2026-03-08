---
description: How to get started
icon: helmet-safety
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/ocz2kFW2PRMWNpWEjhbc/build-system/getting-started
---

# Getting started

If you're familiar with the structure of our build system, navigate to the Structure page or use this as a quick reference. If you're new to our build system, you can get started by reading this first, then diving deep to the build system structure.

To get started with a very minimal C# application with just building and cleaning support, follow these steps (in Visual Studio Code):

1. Follow the steps to create a minimal C# application [here](https://learn.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code). Let's assume that the root C# application name is `NewApp`.
2. Open the terminal by going to **Terminal** > **New Terminal** (or press the <kbd>CTRL</kbd> + <kbd>SHIFT</kbd> + <kbd>\`</kbd> keys)
3. Execute the `git clone https://github.com/Aptivi/tools` command
   1. If you've initialized the Git repository for your project, you'll have to clone it as a submodule inside the root directory of your project using the `git submodule add https://github.com/Aptivi/tools` command and to push the commit.
4. Create the `vendor` directory
5. Underneath the `vendor` directory, create two files:
   1. `vnd_build.py`: This will store commands to initiate the build
   2. `vnd_clean.py`: This will store commands to initiate the cleanup process, such as wiping older build files
6.  Replace the contents of `vnd_build.py` with:

    ```python
    import os
    import subprocess


    def vnd_build(args, extra_args):
        solution = os.path.dirname(os.path.abspath(__file__ + '/../'))
        solution = solution + "/NewApp.slnx"
        command = f"dotnet build {solution} {args if args else ''}"
        result = subprocess.run(command, shell=True)
        if result.returncode != 0:
            raise Exception("Build failed with code %i" % (result.returncode))
    ```
7.  Replace the contents of `vnd_clean.py` with:

    ```python
    import os
    import shutil


    def vnd_clean(extra_args):
        solution = os.path.dirname(os.path.abspath(__file__ + '/../'))
        outputs = {"bin", "obj"}
        directories = get_dirs(solution)
        final_directories = [d.path for d in directories
                             if d.name in outputs]
        for d in final_directories:
            shutil.rmtree(d, ignore_errors=True)


    def get_dirs(directory):
        directories = [d for d in os.scandir(directory) if d.is_dir()]
        for d in list(directories):
            directories.extend(get_dirs(d.path))
        return directories
    ```
8.  Open the terminal emulator on your target platform, change the working directory to the project directory, and execute the build and the clean operations using the following commands:

    ```shellscript
    $ python ./tools/adt.py build
    $ python ./tools/adt.py clean
    ```

If there are no errors, you've created your minimal build system that builds the binary and cleans up old build files!

In order to explore more, take a look at the build system structure to learn more!
