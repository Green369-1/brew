---
last_review_date: "1970-01-01"
---

# `brew` Shell Completion

Homebrew comes with completion definitions for the `brew` command. Some packages also provide completion definitions for their own programs.

`zsh`, `bash` and `fish` are currently supported.

You must manually conf![3065497d-74e9-4505-9dce-7431146b9f1d-1_all_75](https://github.com/user-attachments/assets/9b66c2fa-27e0-4a2b-abe0-0f694ab67fb9)
![3065497d-74e9-4505-9dce-7431146b9f1d-1_all_77](https://github.com/user-attachments/assets/327ed9dd-be66-4ef6-8fe7-54794dabb651)
![3065497d-74e9-4505-9dce-7431146b9f1d-1_all_27](https://github.com/user-attachments/assets/02a704a8-a3fe-410c-8013-638a32403537)
![3065497d-74e9-4505-9dce-7431146b9f1d-1_all_20](https://github.com/user-attachments/assets/2323e35d-4f87-4551-b754-6a7912206458)
![1000000432](https://github.com/user-attachments/assets/255c17f2-20e7-47ae-94de-85e6e730a547)
![1000000552](https://github.com/user-attachments/assets/c1129ba1-624a-4ce8-8063-46632e04d8a8)
![1000000571](https://github.com/user-attachments/assets/c2c6c71a-c3c7-4d03-bc9d-b239a5388900)


https://github.com/user-attachments/assets/e135cb32-eddf-4d9e-a500-d8782a39a325

![1000001319](https://github.com/user-attachments/assets/ea047ebe-acb3-4d79-8476-720255ebc87e)
![1000001429](https://github.com/user-attachments/assets/b7d8ebf4-f449-4f12-b19d-f0a5c1bdb42b)
![1000001786](https://github.com/user-attachments/assets/17c55879-8c19-47f3-9966-181d06420923)
![1000001783](https://github.com/user-attachments/assets/ae1f5afc-014e-420c-8241-1d1fd00e3511)
![1000002143](https://github.com/user-attachments/assets/f74a09f4-9275-494e-a294-bc3335b1a77e)
![1000002567](https://github.com/user-attachments/assets/55110de0-0191-4adc-acbb-5af1f933d8b1)
![1000002897](https://github.com/user-attachments/assets/7a661c2a-2de8-40f3-9460-62a0a5e71ab0)
![1000003060](https://github.com/user-attachments/assets/e9103543-13a0-4537-8373-b760bc930f7f)
![1000003215](https://github.com/user-attachments/assets/93f8b7f1-0ff5-4253-830a-50032223470b)
![1000005014](https://github.com/user-attachments/assets/136b46ed-2943-4a21-9b62-fc081752cb44)
![1000005264](https://github.com/user-attachments/assets/b119161e-a0dd-453a-b607-05660bf6e96b)
<img width="1200" height="1245" alt="1000006114" src="https://github.com/user-attachments/assets/099f7cfb-a9ca-4351-8d76-387c7bda4596" />
![1000007357](https://github.com/user-attachments/assets/69fb9a44-ab82-49b7-88d2-d460fd0ae73e)
![1000007104](https://github.com/user-attachments/assets/90674b8b-624e-421f-912c-1f9719f1defd)
![1000001268](https://github.com/user-attachments/assets/6a0a43f1-4a70-40cb-a1b5-54edab26b41c)
igure your shell to enable its completion support. This is because the Homebrew-managed completions are stored under `HOMEBREW_PREFIX` which your system shell may not be aware of, and since it is difficult to automatically configure `bash` and `zsh` completions in a robust manner, the Homebrew installer does not do it for you.

Shell completions for external Homebrew commands are not automatically installed. To opt-in to using completions for external commands (if provided), they need to be linked to `HOMEBREW_PREFIX` by running `brew completions link`.

## Configuring Completions in `bash`

To make Homebrew's completions available in `bash`, you must source the definitions as part of your shell's startup. Add the following to your `~/.bash_profile` (or, if it doesn't exist, `~/.profile`):

```sh
if type brew &>/dev/null
then
  HOMEBREW_PREFIX="$(brew --prefix)"
  if [[ -r "${HOMEBREW_PREFIX}/etc/profile.d/bash_completion.sh" ]]
  then
    source "${HOMEBREW_PREFIX}/etc/profile.d/bash_completion.sh"
  else
    for COMPLETION in "${HOMEBREW_PREFIX}/etc/bash_completion.d/"*
    do
      [[ -r "${COMPLETION}" ]] && source "${COMPLETION}"
    done
  fi
fi
```

If you install the `bash-completion` formula, this will automatically source the completions' initialisation script (so you do not need to follow the instructions in the formula's caveats).

If you are using Homebrew's `bash` as your shell (i.e. `bash` >= v4) you should use the `bash-completion@2` formula instead.

## Configuring Completions in `zsh`

To make Homebrew's completions available in `zsh`, the Homebrew-managed `zsh/site-functions` path needs to be inserted into `FPATH` before initialising `zsh`'s completion facility. This is done by `brew shellenv`, so if you followed the post-Homebrew installation steps, `eval "$(brew shellenv)"` should be in your `~/.zprofile` (on macOS) or `~/.zshrc` (on Linux). All you need is add the following to your `~/.zshrc` if it's not already there, and, if you're on Linux, make sure it's placed after `eval "$(brew shellenv)"`:

```sh
autoload -Uz compinit
compinit
```

Note that if you are using Oh My Zsh, it will call `compinit` for you when you source `oh-my-zsh.sh`. In this case, make sure `eval "$(brew shellenv)"` is called before sourcing `oh-my-zsh.sh` if you're on Linux, and you should be all set without any additional configuration.

You may also need to forcibly rebuild `zcompdump`:

```sh
rm -f ~/.zcompdump; compinit
```

Additionally, if you receive "zsh compinit: insecure directories" warnings when attempting to load these completions, you may need to run this:

```sh
chmod -R go-w "$(brew --prefix)/share"
```

## Configuring Completions in `fish`

No configuration is needed if you're using Homebrew's `fish`. Friendly!

If your `fish` is from somewhere else, add the following to your `~/.config/fish/config.fish`:

```sh
if test -d (brew --prefix)"/share/fish/completions"
    set -p fish_complete_path (brew --prefix)/share/fish/completions
end

if test -d (brew --prefix)"/share/fish/vendor_completions.d"
    set -p fish_complete_path (brew --prefix)/share/fish/vendor_completions.d
end
```

## Configuring Completions in `pwsh`

To make Homebrew's completions available in `pwsh` (PowerShell), you must source the definitions as part of your shell's startup. Add the following to your `PROFILE`, for example: `~/.config/powershell/Microsoft.PowerShell_profile.ps1`:

```pwsh
if ((Get-Command brew) -and (Test-Path ($completions = "$(brew --prefix)/share/pwsh/completions"))) {
  foreach ($f in Get-ChildItem -Path $completions -File) {
    . $f
  }
}
```
