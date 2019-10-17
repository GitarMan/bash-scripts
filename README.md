# Bash Scripts

In this project I plan to store some simple bash scripts that I want to keep available.

These scripts can be installed in `~/.local/bin/` for general use.

Feel free to use them any way you like!

## FTP Mirror v0.1.0

This script watches a directory recursively using **inotifywait** for file modifications, metadata changes, move files in/out, create, or delete.

Upon detecting a change, it mirrors a local directory to a remote FTP directory using **lftp** based on the settings provided in the configuration file. 

This script requires a JSON configuration file. See example below.

By default it looks for a file named `./ftpmirror.json` in the current local directory, or you can define another file like so: `ftpmirror ~/Documents/Project/config.json`

Here is an example `ftpmirror.json` file:

```json
{
    "PROTOCOL":"sftp",
    "PORT":"2222",
    "URL":"sftp.domain.com",
    "LOCAL_DIR":"/home/user/Documents/Project/sample_directory/",
    "REMOTE_DIR":"/remote/path/sample_directory/",
    "USER":"username",
    "PASS":"password",
    "DELAY":10,
    "MIRROR_OPTIONS":"--continue --delete --verbose=3",
    "INOTIFYWAIT_OPTIONS":"--exclude (secret)|(.(swp|swx)$)"
}
```

### FTP Mirror Usage:

`ftpmirror`
This usage automatically checks for a file called ftpmirror.json in the present working directory.

`ftpmirror ftpmirror-project-name.json`
This usage specifies ftpmirror-project-name.json as the config file.

### FTP Mirror Config Notes:
- **Protocol**: ftp/sftp
- **Port**: 22/2222 (or whichever port your server uses)
- **URL**: Hostname used to connect to your FTP server
- **LOCAL_DIR**: Local path to sync
- **REMOTE_DIR**: Remote path to sync
- **USER**: ftp username
- **PASS**: ftp password
- **DELAY**: Minimum delay in seconds between running lftp mirror (default: 15)
- **MIRROR OPTIONS**: Only change these recommended settings if you have read lftp manual. (--reverse option is hard-coded as this script would not work without it)
-**INOTIFYWAIT OPTIONS**: --exclude {POSIX regular expression} to exclude certain files/patterns from being watched. Read the inotifywait manual for additional options. 

### FTP Mirror Dependencies
- inotifywait
- lftp


## SASSWATCH v0.1.0

This is a simple script that uses **inotifywait** to recursively watch `./sass/` in the present working directory and, when it detects a changed file, it runs **sassc** with the input file of `./sass/style.scss` and the output file of `./style.scss`.

This allows you to work on SASS partials and updates the main style.css file very quickly. I wrote this because I enjoyed the simplicity of being able to quickly compile SASS/SCSS files with **sassc** on my Linux machine (without setting up a task runner like Grunt). However, **sassc** does not currently have a --watch option to watch a directory for changes. This script performs that function for me.  

This reflects my most common project directory structure and suits my needs. If other people find this useful, in the future I may make it more generalized to accept additional arguments such as the SASS directory, output style, input/output file, or other **sassc** options.


### sasswatch Usage:

`sasswatch`
That's it. This script does not yet accept any arguments.


### sasswatch Example Project Directory Structure
.
├── exampledir
├── js
├── **sass**
│  ├── elements
│  ├── mixins
│  ├── modules
│  ├── variables-site
│  └── **style.scss**
└── **style.css**


### sasswatch Dependencies
- inotifywait
- sassc
