# Take A Picture, It'll Last Longer!

[Slides on Speaker Deck][https://speakerdeck.com/psi/take-a-picture-itll-last-longer-goruco-2013]

These are the slides and supporting materials for a [talk][1] I gave at
GoRuCo 2013 in NYC. The talk provides a methodology for using filesystem
snapshotting and cloning features, like those found in ZFS, to allow
developers to branch their databases alongside their source code.

The following directions assume you're on OS X, have installed the [Zevo
Community Edition][2] port of ZFS & have MySQL installed via Homebrew.

**BACK UP YOUR MYSQL DATA BEFORE PROCEEDING, JUST IN CASE**

**BACK UP YOUR MYSQL DATA BEFORE PROCEEDING, JUST IN CASE**

**BACK UP YOUR MYSQL DATA BEFORE PROCEEDING, JUST IN CASE**

1. Create a ZFS filesystem to put your MySQL data on.

   ```
   # Make sure this is big enough to hold your entire database with some
   # extra space. The file can go wherever you prefer.
   $ mkfile 50g /path/to/my_app.vdev

   $ zpool create my_app /path/to/my_app.vdev
   $ zfs create my_app/mysql
   ```

2. Move your MySQL data into ZFS and point your installation at it.
 
   ```
   $ mysql.server stop
   $ mv /usr/local/var/mysql/* /Volumes/my_app/mysql/
   $ rm -rf /usr/local/var/mysql
   $ ln -s /Volumes/my_app/mysql /usr/local/var/mysql
   $ mysql.server start
   ```

3. Put `hooks/post-checkout` into your application's `.git/hooks`
   directory and ensure that it has executable permissions.
4. Switch branches and watch the magic happen.

   ```
   $ git checkout -b new-thing
   Switched to a new branch 'new-thing'
   Shutting down MySQL
   . SUCCESS!
   Creating se_app/mysql-new-thing
   Symlinking /usr/local/var/mysql to /Volumes/se_app/mysql-new-thing
   Starting MySQL
   . SUCCESS!
   $ git checkout master
   Switched to branch 'master'
   Shutting down MySQL
   .. SUCCESS!
   Symlinking /usr/local/var/mysql to /Volumes/se_app/mysql
   Starting MySQL
   . SUCCESS!
   ```

[1]: http://goruco.com/speakers/2013/harrington-jd/
[2]: http://getgreenbytes.com/solutions/zevo/
