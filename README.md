# Pascal's Auto-Rice Install Scripts (PARIS)

Scripts for auto-configuring a fresh Arch Linux install with my dotfiles.

## Installation

After base installation (partitioning, bootloader, basically the [Install page](https://wiki.archlinux.org/index.php/Installation_guide)),
run the following as `root` user:

```sh
curl -LO pascal-riesinger.de/paris.sh
bash paris.sh
```

## The packages.csv list

PARIS will parse the given programs list and install all given programs. Note that the programs file must be a three column .csv.

The first column is a "tag" that determines how the program is installed, "" (blank) for the main repository, A for via the AUR or G if the program is a git repository that is meant to be make && sudo make installed.

The second column is the name of the program in the repository, or the link to the git repository, and the third comment is a description (should be a verb phrase) that describes the program. During installation, PARIS will print out this information in a grammatical sentence. It also doubles as documentation for people who read the csv or who want to install my dotfiles manually.

Depending on your own build, you may want to tactically order the programs in your programs file. PARIS will install from the top to the bottom.

If you include commas in your program descriptions, be sure to include double quotes around the whole description to ensure correct parsing.


## The script itself

The script is broken up extensively into functions for easier readability and trouble-shooting. Most everything should be self-explanitory.

The main work is done by the installationloop function, which iterates through the programs file and determines based on the tag of each program, which commands to run to install it. You can easily add new methods of installations and tags as well.

Note that programs from the AUR can only be built by a non-root user. What PARIS does to bypass this by default is to temporarily allow the newly created user to use sudo without a password (so the user won't be prompted for a password multiple times in installation). This is done ad-hocly, but effectively with the newperms function. At the end of installation, newperms removes those settings, giving the user the ability to run only several basic sudo commands without a password (`shutdown`, `reboot`, `protonvpn-cli`).
