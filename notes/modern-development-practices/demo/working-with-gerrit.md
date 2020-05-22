# Working with Gerrit

This example uses a Gerrit server configured as follows:

* Hostname: 192.168.2.10
* HTTP interface port: 8080
* SSH interface port: 29418

## Change ID

Gerrit needs to identify commits that belong to the same review. For instance, when a change needs to be modified, a second commit can be uploaded to address the reported issues. Gerrit allows attaching those 2 commits to the same change, and relies upon a Change-Id line at the bottom of a commit message to do so. With this Change-Id, Gerrit can automatically associate a new version of a change back to its original review, even across `cherry-picks` and `rebases`.

To be picked up by Gerrit, a Change-Id line must be in the footer (last paragraph) of a commit message, and may be mixed together with Signed-off-by, Acked-by, or other such lines. For example:

```bash
  $ git log -1
  commit 29a6bb1a059aef021ac39d342499191278518d1d
  Author: A. U. Thor <author@example.com>
  Date: Thu Aug 20 12:46:50 2009 -0700

      Improve foo widget by attaching a bar.

      We want a bar, because it improves the foo by providing more
      wizbangery to the dowhatimeanery.

      Bug: #42
      Change-Id: Ic8aaa0728a43936cd4c6e1ed590e01ba8f0fbf5b
      Signed-off-by: A. U. Thor <author@example.com>
      CC: R. E. Viewer <reviewer@example.com>
```

In the above example, `Ic8aaa0728a43936cd4c6e1ed590e01ba8f0fbf5b` is the identity assigned to this change. It is independent of the commit id. To avoid confusion with commit ids, Change-Ids are typically prefixed with an uppercase I.

Note that a Change-Id is not necessarily unique within a Gerrit instance. It can be reused among different repositories or branches

## Making the change

* To get the code run

```bash
git clone ssh://<hostname>:29418/<repository-name>.git
```

* For adding change-id to the commit run

```bash
$ scp -p -P 29418 <sshusername>@<hostname>:hooks/commit-msg <repository-name>/.git/hooks/
$ chmod u+x .git/hooks/commit-msg
```



