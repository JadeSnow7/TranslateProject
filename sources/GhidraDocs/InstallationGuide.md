---
status: collected
title: Ghidra Installation Guide
author: Ghidra Developers Team
collector: Hornos3
collected_date: 20240826
link: https://github.com/NationalSecurityAgency/ghidra/tree/master/GhidraDocs/InstallationGuide.html
---

Ghidra Installation Guide
=========================

The installation information provided is effective as of Ghidra 11.2 and
is subject to change with future releases.

[Platforms Supported](#Platforms)

[Minimum Requirements](#Requirements)

[Installing Ghidra](#Install)

-   [Installation Notes](#InstallationNotes)
-   [Java Notes](#JavaNotes)

[Ghidra Installation Directory Layout](#Layout)

[Building Native Components](#Build)

[Installing the Debuggers\' Python Dependencies](#DebuggerPython)

[Running Ghidra](#Run)

-   [GUI Mode](#RunGUI)
-   [Ghidra Server](#RunServer)
-   [Headless (Batch) Mode](#RunHeadless)
-   [Single Jar Mode](#RunJar)

[Extensions](#Extensions)

-   [Ghidra Extension Notes](#GhidraExtensionNotes)

[Ghidra Development](#Development)

[Upgrade Instructions](#Upgrading)

-   [General Upgrade Instructions](#GeneralUpgrade)
-   [Server Upgrade Instructions](#ServerUpgrade)

[Troubleshooting & Help](#Troubleshooting)

-   [Launching Ghidra](#LaunchIssues)
-   [Using Ghidra](#GhidraHelp)

[Known Issues](#KnownIssues)

-   [All Platforms](#CommonIssues)
-   [Windows](#WindowsIssues)
-   [Linux](#LinuxIssues)
-   [macOS (OS X)](#MacOSIssues)

[]{#Platforms}Platforms Supported
---------------------------------

-   Windows 10 or later (64-bit)
-   Linux (64-bit)
-   macOS 10.13 or later

> **NOTE:** All 32-bit OS installations are now deprecated. Please
> contact the Ghidra team if you have a specific need.

[]{#Requirements}Minimum Requirements
-------------------------------------

### Hardware

-   4 GB RAM
-   1 GB storage (for installed Ghidra binaries)
-   Dual monitors strongly suggested

### Software

Java 21 64-bit Runtime and Development Kit (JDK) (see [Java
Notes](#JavaNotes))

Free long term support (LTS) versions of JDK 21 are provided by:

-   [Adoptium Temurin](https://adoptium.net/temurin/releases)
-   [Amazon
    Corretto](https://docs.aws.amazon.com/corretto/latest/corretto-21-ug/downloads-list.html)

Python3 (3.9 to 3.12; for Debugger support)

-   This is available from [Python.org](https://python.org) or most
    operating system\'s app stores or software repositories. For Linux
    it is recommended that the system\'s package repository be used to
    install a suitable version of Python.

([Back to Top](#top))

### 

[]{#Install}Installing Ghidra
-----------------------------

To install Ghidra, simply extract the Ghidra distribution file to the
desired filesystem destination using any unzip program (built-in OS
utilities, 7-Zip, WinZip, WinRAR, etc)

### []{#InstallationNotes}Installation Notes

-   Ghidra does not use a traditional installer program. Instead, the
    Ghidra distribution file is simply extracted in-place on the
    filesystem. This approach has advantages and disadvantages. On the
    up side, administrative privilege is not required to install Ghidra
    for personal use. Also, because installing Ghidra does not update
    any OS configurations such as the registry on Windows, removing
    Ghidra is as simple as deleting the Ghidra installation directory.
    On the down side, Ghidra will not automatically create a shortcut on
    the desktop or appear in application start menus.
-   When launching Ghidra for the first time on macOS, the macOS
    Gatekeeper feature may attempt to quarantine the pre-built unsigned
    Ghidra native components. Two techniques can be used to prevent this
    from happening:
    -   Prior to extracting the Ghidra distribution file, running *xattr
        -d com.apple.quarantine ghidra\_\<version\>\_\<date\>.zip* from
        a terminal.
    -   Prior to first launch, following the instructions in the
        [Building Native Components](#Build) section.
-   Administrative privilege may be required to extract Ghidra to
    certain filesystem destinations (such as C:\\), as well as install
    the Ghidra Server as a service.
-   Ghidra relies on using directories outside of its installation
    directory to manage both temporary and longer-living cache files.
    Ghidra attempts to use standard OS directories that are designed for
    these purposes in order to avoid several issues, such as storing
    large amounts of data to a roaming profile. If it is suspected that
    the default location of these directories is causing a problem, they
    can be changed by modifying the relevant properties in the
    *support/launch.properties* file.

### []{#JavaNotes}Java Notes

-   Ghidra requires a [supported](#Requirements) version of a Java
    Runtime and Development Kit on the PATH to run. However, if there is
    a version of Java on the PATH that Ghidra does not support, it will
    use that version of Java (if 1.8 or later) to assist in locating a
    supported version on your system. If one cannot be automatically
    located, the user will be prompted to enter a path to the Java home
    directory to use (the Java home directory is the parent directory of
    Java\'s *bin* directory). This minimizes the impact Ghidra has on
    pre-existing configurations of Java that other software may rely on.
-   Depending on your operating system, it may be possible to find and
    install a [supported](#Requirements) version of a Java Runtime and
    Development Kit through your package manager, without the need to
    set the Path environment variables as described below.
-   If Ghidra failed to run because *no versions* of Java were on the
    PATH, a [supported](#Requirements) JDK should be installed via a
    Linux package manager (aptitude, yum, etc), Windows installer
    program (\*.exe, \*.msi), macOS Installer package (\*.pkg), or
    manually extracted and added to the PATH. The following steps
    outline how to manually extract and add a JDK distribution to the
    operating system\'s PATH.
    -   **Windows:** Extract the JDK distribution (.zip file) to your
        desired location and add the JDK\'s bin directory to your PATH:

        1.  Extract the JDK:

            1.  Right-click on the zip file and click **Extract
                All\...**
            2.  Click **Extract**

        2.  Open Environment Variables window:

            1.  Right-click on Windows start button, and click
                **System**
            2.  Click **Advanced system settings**
            3.  Click **Environment variables\...**

        3.  Add the JDK bin directory to the PATH variable:

            1.  Under **System variables**, highlight *Path* and click
                **Edit\...**
            2.  At the end of the the *Variable value* field, add a
                semicolon followed by *\<path of extracted JDK
                dir\>\\bin*, or use the **New** button in the *Edit
                environment variable* window to add a new entry to the
                *Path*.
            3.  Click **OK**
            4.  Click **OK**
            5.  Click **OK**

        4.  Restart any open Command Prompt windows for changes to take
            effect

    -   **Linux and macOS (OS X):** Extract the JDK distribution
        (.tar.gz file) to your desired location, and add the JDK\'s bin
        directory to your PATH:

        1.  Extract the JDK:

            > *tar xvf \<JDK distribution .tar.gz\>*

        2.  Open \~/.bashrc with an editor of your choice. For example:

            > *vi \~/.bashrc*

        3.  At the very end of the file, add the JDK bin directory to
            the PATH variable:

            > *export PATH=\<path of extracted JDK dir\>/bin:\$PATH*

        4.  Save file

        5.  Restart any open terminal windows for changes to take effect
-   In some cases, you may want Ghidra to launch with a specific version
    of Java instead of the version that Ghidra automatically locates. To
    force Ghidra to launch with a specific version of Java, set the
    **JAVA\_HOME\_OVERRIDE** property in the *support/launch.properties*
    file. If this property is set to an incompatible version of Java,
    Ghidra will revert to automatically locating a compatible version.
    Note that *some* Java must still be on the PATH in order for Ghidra
    to use the **JAVA\_HOME\_OVERRIDE** property. This limitation will
    be addressed in a future version of Ghidra.

([Back to Top](#top))

[]{#Layout}Ghidra Installation Directory Layout
-----------------------------------------------

When Ghidra is installed, the runnable software gets extracted to a new
directory we will refer to as *\<GhidraInstallDir\>*. Below is a
description of the top-level directories and files that can be found in
*\<GhidraInstallDir\>* once extraction of the distribution file is
complete.

  --------------------- ------------------------------------------------------------
  **Ghidra**            Base directory for Ghidra distribution.  Contains files
                        needed to run Ghidra.

  **Extensions**        Optional components that can extend Ghidra\'s functionality
                        and integrate Ghidra with other tools.\
                        See the [Extensions](#Extensions) section for more
                        information.

  **GPL**               Standalone GPL support programs.

  **server**            Contains files related to Ghidra Server installation and
                        administration.

  **support**           Contains files useful for debugging Ghidra, running Ghidra
                        in advanced modes, and controlling how Ghidra launches.

  **docs**              Contains documentation for Ghidra, such as release notes,
                        API files, tutorials, etc.

  **ghidraRun(.bat)**   Script used to launch Ghidra.

  **LICENSE.txt**       Ghidra license information.

  **licenses**          Contains licenses used by Ghidra.

  **bom.json**          Software Bill of Materials (SBOM) in CycloneDX JSON format.
  --------------------- ------------------------------------------------------------

([Back to Top](#top))

[]{#Build}Building Native Components
------------------------------------

Ghidra requires several native binaries to be present in order to
successfully run. An official public Ghidra release includes native
binaries for the following platforms.

NOTE: For some non-public Ghidra releases macOS natives may be omitted.

-   Windows 10 or later, x86 64-bit
-   Windows 10 or later, ARM 64-bit (using x86 emulation)
-   Linux x86 64-bit
-   macOS x86 64-bit
-   macOS ARM 64-bit

Ghidra supports running on the following additional platforms with
user-built native binaries:

-   Linux ARM 64-bit
-   FreeBSD x86 64-bit (no debugger support)
-   FreeBSD ARM 64-bit (no debugger support)

For supported systems where native binaries have not been supplied, or
those that are supplied fail to run properly, it may be necessary to
build the native Ghidra binaries. In order to build native binaries for
your platform, you will need the following installed on your system:

A [supported](#Requirements) version of a Java Development Kit

[Gradle 8.5+](https://gradle.org/releases/) (or supplied Gradle wrapper
with Internet connection)

Software C/C++ build tools and library packages

-   macOS: *Xcode* or the abbreviated *Command Line Tools for Xcode*
    will supply the necessary build tools. Assuming you are connected to
    the Internet, *Xcode* (which includes the command tools) may be
    installed directly from the App Store while *Command Line Tools for
    Xcode* may be installed using the following command:

            xcode-select --install

-   Linux/FreeBSD: the 64-bit versions of the following packages should
    installed:
    -   gcc 8.5 or later
    -   gcc-c++ / g++ 8.5 or later
    -   make

-   Windows: [Microsoft Visual
    Studio](https://visualstudio.microsoft.com/vs/community/) 2017 or
    later, or [Microsoft C++ Build
    Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
    with the following components installed:
    -   MSVC
    -   Windows SDK
    -   C++ ATL

To build the native binaries for your current platform, execute the
following commands:

>         cd <GhidraInstallDir>/support/gradle/
>         gradle buildNatives
>       

If you are connected to the Internet and do not have Gradle installed,
execute:

>         cd <GhidraInstallDir>/support/gradle/
>         ./gradlew(.bat) buildNatives
>       

When the commands successfully complete, Ghidra will contain newly built
native binaries in the relevant modules\' *build/os/\<platform\>/*
subdirectories, which will override any existing pre-built native
binaries in the *os/\<platform\>/* subdirectories.

([Back to Top](#top))

[]{#DebuggerPython}Installing the Debugger\'s Python Dependencies
-----------------------------------------------------------------

The Debugger now uses Python to connect to the host platform\'s native
debuggers. This requires Python3 (3.9 to 3.12) and some additional
packages. These packages are included in the distribution, but you may
still install them from PyPI if you prefer:

-   psutil
-   protobuf==3.20.3
-   Pybag\>=2.2.10 (for WinDbg support)

Different native debuggers have varying requirements, so you do not
necessarily have to install all of the above packages. Each connector
will inform you of its specific requirements and where they must be
installed. In some cases, you may need to install packages on the target
system.

[]{#Run}Running Ghidra
----------------------

### []{#RunGUI}GUI Mode

1.  Navigate to *\<GhidraInstallDir\>*

2.  Run *ghidraRun.bat* (Windows) or *ghidraRun* (Linux or macOS)

    If Ghidra failed to launch, see the
    [Troubleshooting](#Troubleshooting) section.

### []{#RunServer}Ghidra Server

Ghidra can support multiple users working together on a single project.
Individual Ghidra users launch and work on their own local copies of a
particular Ghidra project but check changes into a common repository
containing all commits to that repository. For detailed information on
installing/configuring the Ghidra Server see the
*\<GhidraInstallDir\>*/server/svrREADME.html file.

### []{#RunHeadless}Headless (Batch) Mode

Ghidra is traditionally run in GUI mode. However, it is also capable of
running in headless batch mode using the command line. For more
information, see the
*\<GhidraInstallDir\>*/support/analyzeHeadlessREADME.html file.

### []{#RunJar}Single Jar Mode

Normally, Ghidra is installed as an entire directory structure that
allows modular inclusion or removal of feature sets and also provides
many files that can be extended or configured. However, there are times
when it would be useful to have all or some subset of Ghidra compressed
into a single jar file at the expense of configuration options. This
makes Ghidra easier to run from the command line for headless operation
or to use as a library of reverse engineering capabilities for another
Java application.

A single ghidra.jar file can be created using the
*\<GhidraInstallDir\>*/support/buildGhidraJar script.

([Back to Top](#top))

[]{#Extensions}Extensions
-------------------------

Extensions are optional components that can:

-   Extend Ghidra\'s functionality with experimental or user-contributed
    Ghidra plugins or analyzers.
-   Integrate other tools with Ghidra, such as Eclipse or IDAPro.

Ghidra comes with the following extensions available for use (and by
default uninstalled), which can be found in the
*\<GhidraInstallDir\>*/Extensions directory.

-   **Eclipse:** The GhidraDev Eclipse plugin for a pre-existing Eclipse
    installation. For information on installing and using the GhidraDev
    Eclipse plugin, see
    *\<GhidraInstallDir\>*/Extensions/Eclipse/GhidraDev/GhidraDev\_README.html.
-   **Ghidra:** Ghidra extensions (formerly known as contribs). See
    [Ghidra Extension Notes](#GhidraExtensionNotes) for more
    information.
-   **IDAPro:** IDAPro plugins/loaders for transferring items with
    Ghidra.

### []{#GhidraExtensionNotes}Ghidra Extension Notes

-   Ghidra extensions are designed to be installed and uninstalled from
    the Ghidra front-end GUI:

    1.  Click **File → Install Extensions**
    2.  Check boxes to install extensions; uncheck boxes to uninstall
        extensions
    3.  Restart Ghidra for the changes to take effect

-   Extensions installed from the Ghidra front-end GUI get installed at
    *\<UserDir\>*/.ghidra/.ghidra-\[version\]/Extensions.

-   It is possible to install Ghidra extensions directly into the Ghidra
    installation directory. This may be required if a system
    administrator is managing extensions for multiple users that all use
    a shared installation of Ghidra. It may also be more convenient to
    manage extensions this way if a Ghidra installation is only ever
    used headlessly.

    To install an extension in these cases, simply extract the desired
    Ghidra extension archive file(s) to the
    *\<GhidraInstallDir\>*/Ghidra/Extensions directory. For example, on
    Linux or macOS:

    1.  Set current directory to the Ghidra installed-extensions
        directory:
    2.  Extract desired extension archive file(s) to the current
        directory:
    3.  The extension(s) will be installed the next time Ghidra is
        started.

    To uninstall extensions, simply delete the extracted extension
    directories from *\<GhidraInstallDir\>*/Ghidra/Extensions. The
    extension(s) will be uninstalled the next time Ghidra is started.

    **NOTE:** It may not be possible to uninstall an extension in this
    manner if there is an instance of Ghidra running that holds a file
    lock on the extension directory that is trying to be deleted.

([Back to Top](#top))

[]{#Development}Ghidra Development
----------------------------------

Users can extend the functionality of Ghidra through the development of
custom Ghidra scripts, plugins, analyzers, etc.

Ghidra supports development in Eclipse by providing a custom Eclipse
plugin called **GhidraDev**, which can be found in the
*\<GhidraInstallDir\>*/Extensions/Eclipse directory. For more
information on installing and using the GhidraDev Eclipse plugin, see
*\<GhidraInstallDir\>*/Extensions/Eclipse/GhidraDev/GhidraDev\_README.html.

**NOTE:** Eclipse is not provided with Ghidra. The GhidraDev Eclipse
plugin is designed to be installed in a pre-existing Eclipse
installation.

Ghidra scripting API javadocs can be found at
*\<GhidraInstallDir\>*/docs/GhidraAPI\_javadoc.zip.

([Back to Top](#top))

[]{#Upgrading}Upgrade Instructions
----------------------------------

### []{#GeneralUpgrade}General Upgrade Instructions

**!!!Important!!! BACKUP YOUR OLD PROJECTS FIRST!! !!!Important!!!**

-   Backup by manually copying the *.rep* directory and *.gpr* file from
    any Ghidra project directories to a safe location on your file
    system.

New installations of Ghidra will, by default, use the saved profile from
a user\'s most recent version of Ghidra. This allows any saved tool
configurations to be automatically ported to new projects. However, this
may also prevent new tool options and features from automatically being
configured in some cases. To open new tools containing the latest
configurations, users should, from the Project Manager Window, choose
**Tools → Default Tools\...**

When you open a program that was created using a previous version of
Ghidra, you will be prompted to upgrade the program before it can be
opened. The upgrade will not overwrite your old file until you save it.
If you save it (to its original file), you will no longer be able to
open it using an older version of Ghidra. You could, however, choose to
perform a "Save As" instead, creating a new file and leaving the old
version unchanged. Be very careful about upgrading shared program files
since everyone accessing the file must also upgrade their Ghidra
installation.

### []{#ServerUpgrade}Server Upgrade Instructions

-   Please refer to the*\<GhidraInstallDir\>*/server/svrREADME.html file
    for details on upgrading your Ghidra Server.

([Back to Top](#top))

[]{#Troubleshooting}Troubleshooting & Help
------------------------------------------

### []{#LaunchIssues}Launching Ghidra

When launching Ghidra with the provided scripts in
*\<GhidraInstallDir\>* and *\<GhidraInstallDir\>*/support, you may
encounter the following error messages:

-   **Problem:** *Java runtime not found.*
    -   **Solution:** A Java runtime (java/java.exe) is required to be
        on the system PATH. Please see the [Requirements](#Requirements)
        section for what version of Java must be pre-installed for
        Ghidra to launch.
-   **Problem:** *Failed to find a supported JDK.*
    -   **Solution:** The Ghidra launch script uses the Java runtime on
        the system PATH to find a supported version of a Java
        Development Kit (JDK) that Ghidra needs to complete its launch.
        Please see the [Requirements](#Requirements) section for what
        version of JDK must be pre-installed for Ghidra to launch.
-   **Problem:** *Exited with error. Run in foreground (fg) mode for
    more details.*
    -   **Solution:** Ghidra failed to launch in the background and the
        error message describing the cause of the failure is being
        suppressed. Rerun Ghidra in the foreground by setting the
        LAUNCH\_MODE variable in the launch script you ran to **fg**.
        Alternatively, you can use the
        *\<GhidraInstallDir\>*/support/ghidraDebug script to run Ghidra
        in debug mode, which will also allow you to see the error
        message as well as additional debug output. **NOTE:** By
        default, running Ghidra in debug mode listens on
        127.0.0.1:18001.

### []{#GhidraHelp}Using Ghidra

There are several ways you can get help with using Ghidra:

-   Tutorials and other documentation can be found in
    *\<GhidraInstallDir\>*/docs.
-   When Ghidra is running, extensive context sensitive help is
    available on many topics.  To access Help on a topic, place your
    mouse on a window, menu or component and press \<F1\>.  Help for
    that window/menu/component will be displayed.
-   When Ghidra is running, indexed help can be found under **Help →
    Topics\...**

([Back to Top](#top))

[]{#KnownIssues}Known Issues for current release

### []{#CommonIssues}All Platforms

-   Displaying the correct processor manual page for an instruction
    requires the installation of Adobe Reader 8.0.x or later.  Adobe
    broke the goto page in Reader version 7.x.  If a newer version of
    Reader is not installed, then the manual for the processor will
    display at the top of the manual.  Using an Adobe Reader version
    later than 8.0.x works for most platforms, but some platforms and
    version of the reader still have issues.
-   Some actions may block the GUI update thread if they are long
    running.
-   Project archives only store private and checked out files within the
    archive.  Project archives do not support server-based repositories.
-   When using a Ghidra server, all clients and the server must have a
    valid Domain Name Server (DNS) defined which has been properly
    configured on the network for both forward and reverse lookups.
-   Image base may not be changed to an address which falls within an
    existing memory block.
-   Language versioning and migration does not handle complex changes in
    the use of the context register.
-   Ghidra will not launch when its path contains a \"!\" character.
    This is to avoid issues that Java\'s internal libraries have parsing
    these paths (\"!\" is used as a jar-separator by Java).

### []{#WindowsIssues}Windows

-   Older versions of 7-Zip may not be able to unpack the Ghidra
    distribution file if it contains any files with a 0-byte length.
    Upgrade to a newer version of 7-Zip to fix this problem.
-   Ghidra will fail to launch when its path contains a \"\^\"
    character.

### []{#LinuxIssues}Linux

-   Ghidra may not display correctly when run from a Linux remote
    desktop session that uses 32-bit color depth. Setting the remote
    desktop application\'s color depth to 24-bit has been known to
    improve this issue.
-   Some users have reported Ghidra GUI rendering issues on
    multi-monitor thin client setups. These problems are attributed to
    reported bugs in Java, which will hopefully be fixed in the future.
    Disabling the 2nd or 3rd monitor may be necessary to work around the
    issue.
-   GUI icons may not render correctly in some configurations of Linux.
    Setting *VMARGS=-Dsun.java2d.opengl* to **true** in
    *\<GhidraInstallDir\>*/support/launch.properties may fix this issue.

### []{#MacOSIssues}macOS (OS X)

-   Building new Ghidra module extensions on macOS (OS X) using a
    network drive (including a network-mapped home directory) throws a
    Java exception. This issue is known to the Java/macOS community but
    a fix has not yet been released. See
    *\<GhidraInstallDir\>*/Extensions/Eclipse/GhidraDev/GhidraDev\_README.html
    for more information on building Ghidra module extensions from
    Eclipse.

([Back to Top](#top))

\
\
\