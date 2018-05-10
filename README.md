# set-up-my-bpro
Initial setup for a Banana Pi Pro using `Vagrant` and `Ansible`.

# still to do
testing:
    execute roles twice
    check every role on its own
    check, if packages are installed in their own role and not from "common" (see motd, sudo)

    fix postfix "header_checks" and so on
    adapt ntp.conf of Gentoo

set cpu govenor to "conservative": /etc/default/cpufrequtils


!! use "lynis" to test security and stuff !!

document usage and make "README.md" fancy

!! find another way to manipulate variables, current solution is to set them globally in "set_up_armbian.yml" ("import_role" with "vars" does not work at the moment: https://github.com/ansible/ansible/issues/18341)
# Dotfile switches
/usr/local/etc/.ansible_dotfile_switches
dynamic_motd_content_set
fastest_mirror_set
host_keys_generated
hostname_set
ssh_banner_content_set
sshd_settings_local_test_set
static_motd_content_set
tmux_mem_cpu_load_compiled
webui_aria2_repository_content_copied

# Ansible conventions
The following conventions were used to create all roles:

use devel version only
use ssh pipelining and not the deprecated accelerate option!!
when cloning with "git" module, always use 'force: "yes"'
be idempotent: try to get "ok"s when executing a second time
avoid using the "shell" module as much as possible to keep idempotency
write custom variables in uppercase
add a trailing slash, if path ends with a directory name or name is a directory - except for symlink destinations
use absolute paths everywhere
use "dot_" as file name prefix when using dotfiles
use "$(command -v <command>)" when using "shell" module
when using varaibles in ansible vault, use "VAULT_" as prefix
((((use "command" module whenever possible to execute commands. in other cases where something needs to be piped, use the "shell" module))))
to sanitize any variables passed to the shell module, use "{{ var | quote }}" instead of just "{{ var }}" to make sure they don't include evil things like semicolons.
mirror remote directory structure in roles (files/etc/zsh, templates/etc/apache2, ...)
add tags at the end of every task: role name is the first tag (more follows, if it makes sense)
use "meta" directory to include dependencies
use "include_*" module when using conditionals with "when" and "import_*", if not
make shell module idempotent by creating dotfiles in directory "/usr/local/etc/" and using "creates", if it does make sense
assign new variables when using "meta/main.yml" for respective role
reload daemons if possible. if not, restart them
always use lists when writing "tags" or "when" statements
copy single files with "copy" module and a lot of file with "synchronize" module
write task names and comments in lowercase
write task names without quotes and emphasise keywords with quotes
task names should only be less than 80 characters long
if compiling is necessary, do every step as separate task (makes debugging easier)
write role names in quotes
always execute "*_global.yml" playbooks at the beginning within a "main.yml" file
always use "with_items" when creating subdirectories with "file" module to improve readability
always break "with_items" objects to improve readability
when having multiple objects in "with_items", use "<MODULE_NAME>_<MODULE_OPTION>:" as variables for each option
describe as distinct as possible
always write states at the bottom of a task
empty variables in "defaults/main.yml" when invoking the role with a parameter (e.g. - { role: "somerole", VARIABLE: "something" })
use new syntax by using newline and not equal signs!
tasks: always use "main.yml" to include other yml files, also set variables for met conditions
roles: use underscore as delimiter
when installing multiple packages, do not use "with_items", use a yml list instead. otherwise every package gets installed one by one which is very slow
default values: avoid changing them as much as possible, look for a way to have custom settings included or look for for directories which were made for that
when uncommenting lines use "replace" module instead of "lineinfile" to avoid redundant options
installing: <write what it does> (<package_name>) if there are multiple packages, do not name any package name
writing into files: check, if a file at "/opt/.<something>_content_set" exists, do stuff with file, if check is false, touch a file "/opt/.<something>_content_set"
when copying templates always use "# {{ ansible_managed }}" at the top of the file to indicate that the file was copyied by ansible and should not be edited without thought
write custom scripts in bash
installing: immediately call "enable" and "restart" handler, if there is a way to control the daemon
all modules: use "validate" module as much as possible to keep consistency
"replace" and "lineinfile" module: always replace exact expression with other value
when writing regular expressions: use double quotes. if necessary, use single quotes
when writing script: log execution in "/var/log/syslog"
when using cron: do not copy shell scripts to "/etc/cron.*", use "crontab -e" with its respective user
