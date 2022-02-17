### docker-compose error /bin/bash^M: a bad interpreter: No such file or directory  (Windows OS)
Unix uses different line endings so can't read the file you created on Windows.  
Hence, it is seeing `^M` as an illegal character.  
That's the reason container crushes while bash script executing inside Docker container with Linux inside.

1. Solution 1  
Open bash script with Notepad++ (or other text editor you are using)  
Find: `Edit -> EOL Conversion`   
Choose `Unix(LF)`
Save changes  
   
2. Solution 2  
   Regarding [GitHub Documentation](https://docs.github.com/en/github/getting-started-with-github/configuring-git-to-handle-line-endings#example)
    The git config core.autocrlf command is used to change how Git handles line endings. It takes a single argument.

    On Windows, you simply pass true to the configuration. For example:
    ```shell
    $ git config --global core.autocrlf true
    # Configure Git to ensure line endings in files you checkout are correct for Windows.
    # For compatibility, line endings are converted to Unix style when you commit files.  
    ```
    Optionally, you can configure a `.gitattributes` file to manage how Git reads line endings in a specific repository.  
    When you commit this file to a repository, it overrides the core.autocrlf setting for all repository contributors. This ensures consistent behavior for all users, regardless of their Git settings and environment.
    
    The `.gitattributes` file must be created in the root of the repository and committed like any other file.
    
    Here's an example `.gitattributes` file:
    
    ```shell
    # Declare files that will always have CRLF line endings on checkout.
    *.sh text eol=lf
    ```
    
    `text eol=lf` Git will always convert line endings to LF on a checkout.  
    You should use this for files that must keep LF endings, even on Windows.
   
    Refresh a repository after changing line endings like [described in documentation](https://docs.github.com/en/github/getting-started-with-github/configuring-git-to-handle-line-endings#refreshing-a-repository-after-changing-line-endings)
