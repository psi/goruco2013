1. Make sure you have ZFS. On OS X, you can install [Zevo][1].
2. Create a ZFS filesystem to put your MySQL data on.

   ```
   # Make sure this is big enough to hold your entire database with some
   # extra space. The file can go wherever you prefer.
   $ mkfile 50g /path/to/my_app.vdev

   $ zpool create my_app /path/to/my_app.vdev
   $ zfs create my_app/mysql
   ```

3. Move your MySQL data into ZFS and point your installation at it. The
   following assume you've installed MySQL via Homebrew.
 
   ```
   $ mysql.server stop
   $ mv /usr/local/var/mysql/* /Volumes/my_app/mysql/
   $ rm -rf /usr/local/var/mysql
   $ ln -s /Volumes/my_app/mysql /usr/local/var/mysql
   $ mysql.server start
   ```

4. Put `hooks/post-checkout` into your application's `.git/hooks`
   directory and ensure that it has executable permissions.
5. Switch branches and watch the magic happen.

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

[1]: http://getgreenbytes.com/solutions/zevo/
