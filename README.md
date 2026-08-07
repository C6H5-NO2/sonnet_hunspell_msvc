# sonnet_hunspell_msvc

This repo contains a workflow to compile [Sonnet](https://invent.kde.org/frameworks/sonnet) with [Hunspell](https://github.com/hunspell/hunspell) enabled using MSVC 2022.


## Background

Sonnet supports various plugins for spell checking. On Windows, it defaults to [ISpellChecker](https://learn.microsoft.com/en-us/windows/win32/api/spellcheck/nn-spellcheck-ispellchecker). However, this Win32 API requires a per-language, system-wise installation of huge language features, which also limits the choice of languages and dictionaries. Hunspell, on the other hand, is open-source, not integrated with the whole OS, and compatible with any dic + aff files.

Sadly, the current 26.05 [Craft cache](https://files.kde.org/craft/Qt6/26.05/windows/cl/msvc2022/x86_64/RelWithDebInfo/kde/frameworks/tier1/sonnet/) only ships `sonnet_ispellchecker.dll` on Windows regardless of the compiler, unlike the Linux package getting `sonnet_hunspell.so` out of the box. It seems to be a [deliberate decision](https://invent.kde.org/packaging/craft-blueprints-kde/-/blob/master/kde/frameworks/tier1/sonnet/sonnet.py?ref_type=heads#L12) of the developers.

As a result, this repo is here to build `sonnet_hunspell.dll` and `libhunspell.dll` for Windows users.


## Usage

The workflow is meant to run on the [Windows Server 2022](https://github.com/actions/runner-images/blob/main/images/windows/Windows2022-Readme.md) image. Though not recommended nor tested, one may manually execute the shell commands in a local Windows PowerShell.

1. Install Visual Studio 2022 and the C++ workload.
2. Install [KDE Craft](https://invent.kde.org/packaging/craft).
3. Run `craft hunspell; if ($?) { craft --options kde/frameworks/tier1/sonnet.useHunspell=True sonnet }`.
4. Find the DLLs under `$env:CraftRoot`.
