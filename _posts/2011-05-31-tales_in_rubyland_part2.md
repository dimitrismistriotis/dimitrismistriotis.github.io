---
layout: default
title: Tales in Rubyland part 2 - "You can write elegant shell scripts and unit test them too".
permalink: /posts/tales_in_rubyland_part2
---

This is part 1:<a title="How to learn Ruby – Tales in Rubyland part 1" href="/posts/tales_in_rubyland_part1">How to learn Ruby – Tales in Rubyland part 1</a>.

Recently I saw a great presentation on a "sister" technology to Rails, Grails. Although many people are using modern scripting languages for building  web applications, I believe that there is an area which has strangely become  overlooked: service automation and ... scripts.

Recently there was the need to do the following (presented in reverse order):


## Classic Shell scripting

In a server I am maintaining, there is a process that creates every day a specific archive of some folders on a server, this archive should be send to a remote location.

Out of the many ways to handle this, I chose to email that file to a gmail account. I had already access to a mail server (actually, a google-apps mail box), so the job is being done by a script that looks like this:

<pre lang="ruby">require 'rubygems'
require 'gmail'
now_time = Time.now.strftime("%Y-%m-%d") # naming convention for files
backup_folder = "/path/to/backups/" + now_time + "/"
backup_file = backup_folder + now_time + '.tgz'
gmail = Gmail.new('backupd@GOOGLEAPPSDOMAIN', 'I_KNOW_HARD_CODING_A_PASSWORD_IS_WRONG')
gmail.deliver do
to "first_recepient@GOOGLEAPPSDOMAIN, second-receipient@GOOGLEAPPSDOMAIN"
subject "GOOGLEAPPSDOMAIN Backup of #{now_time}"
text_part do
body "Automated backup of GOOGLEAPPSDOMAIN in tgz format"
end
add_file backup_file
end
exit</pre>

Which is then appended to the appropriate user's crontab.

The benefits of this in comparison to other method's I have seen in use is  mainly the extensive readability and therefore maintainability of this script. On  a typical bash scheme, there would be the need to write something like:
<pre lang="bash">mail -s "aaa" - u "bbb" --body=="I don't know" #etc</pre>
In this case even a person who does not even know Ruby can maintain it, for example: add a third recipient.

## Unit testing shell scripts AKA "You used Ruby to do what?"

After seeing how these archives are being dispatched, let's look on how they are created

<pre lang="ruby">class BackupDbCron
def get_dump_location prefix_path, time_suffix
prefix_path   += "/" unless prefix_path[-1, 1] == "/"
return prefix_path + time_suffix + "/db_dump.sql"
end
def get_error_location prefix_path, time_suffix
prefix_path   += "/" unless prefix_path[-1, 1] == "/"
return prefix_path + time_suffix + "/db_dump.error"
end
def get_execution_string executable, prefix_path, time_suffix, database_name
return  executable + \\
" -uXXX -pYYY" + \\
" " + database_name + \\
" &gt;" + get_dump_location(prefix_path, time_suffix) + \\
" 2&gt;" + get_error_location(prefix_path, time_suffix)
end
def backup_db mysqdump, prefix_path, time_suffix, database_name
execution_script = self.get_execution_string mysqdump, prefix_path, time_suffix, database_name
`#{execution_script}`
end
end
</pre>

Which creates a typical call to mysqldump. Usually in order to make this "right" I had to experiment with calls to
"date" on the shell and a big number of "echo"s which later had to be removed. Sounds a bit familiar?

Now thanks to Ruby's ecosystem, it was possible to do this:

<pre lang="ruby">require './BackupDbCron.rb'
require "test/unit"
class BackupDbCronTest &lt; Test::Unit::TestCase
def test_correct_target_dump_method
db_backup = BackupDbCron.new
assert_respond_to(db_backup, "get_dump_location")
end
def test_correct_target_folder_with_slashes
db_backup = BackupDbCron.new
assert_equal("/backupfolder/2011-01-01/db_dump.sql", db_backup.get_dump_location("/backupfolder/", "2011-01-01"))
end
... etc ...
def test_get_execution_string
db_backup = BackupDbCron.new
assert_respond_to(db_backup, "get_execution_string")
assert_equal("mysqldump -uXXX -pYYY DBNAME &gt;/backupfolder/2011-01-01/db_dump.sql 2&gt;/backupfolder/2011-01-01/db_dump.error",
db_backup.get_execution_string("mysqldump", "/backupfolder", "2011-01-01", "DBNAME"))
end
end
</pre>

This is the first time that I came across this and there are many things that could be done to extend it. It would be handy to mock the call to shell and conduct tests on that. But for me it was a bit of a revelation. I did not know that shell scripts could be unit tested. The advantage of this approach is that things that usually required an hour, now could be done in 20 to 30 minutes. Additionally it was very easy to change some stuff in a later stage because after introducing failing tests, the way to proceed was almost obvious.
