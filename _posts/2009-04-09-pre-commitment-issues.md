---
title: "Subversion: Pre-commitment issues"
excerpt_separator: <!--more-->
---
I've been doing a bit of Subversion hackery recently, and thought I'd briefly cover some of the major moving parts here for future reference.

First things first - Subversion hooks go into the ```/hooks``` directory of your repository and are executed by Subversion at various stages of the Subversion process. I'm working on a pre-commit hook, which will be executed by Subversion before each check-in is committed to the repository. Any non-zero return code from the hook will cause Subversion to deny the check-in.
<!--more-->

If your Subversion repository is hosted on Unix, all you need to do is to name your pre-commit script "pre-commit" (note no extension), and make sure it is executable and has the appropriate hashbang at the top.

If your repository is hosted on Windows (like my test-repo is), you'll need to create a ```pre-commit.bat``` file and call through to your script, like this -

```batch
@rem PRE-COMMIT.BAT
@rem
@rem Pre-commit script for Subversion running on Windows.
@rem Calls a pre-commit python script in the current folder.

pre-commit.py %* 
```

(I'm obviously relying on the ```.py``` extension being correctly associated with the Python interpreter here, but you could also invoke the interpreter directly if you wanted to.)

Now you have the basics in place, every check-in to the repo will cause the ```pre-commit.bat``` to be executed. However, this leaves you with a problem when it comes to debugging - the only way to execute your script is by attempting to make a commit to the repo and wait for Subversion to call the script.

Enter this cunning little trick from [Thomas Guest](http://wordaligned.org/articles/a-subversion-pre-commit-hook) - executing your hook script from the command-line with a past revision:

```python
#!/usr/local/bin/python

"""
Pre-commit code goes here. Non-zero return code indicates
failure.
"""
def preCommit( repo, transaction, useRevision=False ):
        # put your pre-commit code here
       
        return 0

"""
Main control logic. Parses the command-line and invokes the
preCommit() method with the appropriate parameters.
"""
def main():
        usage = """usage: %prog [options] REPO TXN

Run pre-commit checks on a repository transaction.

For debugging, try "%prog -r /path/to/repo 123" to check revision 123
"""
        from optparse import OptionParser
        parser = OptionParser(usage=usage)
        parser.add_option(
                "-r", "--revision",
                help="Test mode. TXN actually refers to a revision.",
                action="store_true", default=False)

        try:
                (opts, (repo, transaction)) = parser.parse_args()
        except:
                parser.print_help()
                return 1

        return preCommit( repo, transaction, opts.revision )

if __name__ == "__main__":
        import sys
        sys.exit(main())
```

When Subversion invokes the hook, it will call it with the repository-location as the first parameter, and a transaction id as the second. By allowing the command-line to treat the 2nd parameter as a revision instead, it allows us to test and debug the hook without going through Subversion all the time:

```pre-commit.py -r /path/to/repo 123```

Thanks to Thomas for the idea, and apologies for reimplementing it badly.