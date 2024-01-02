

# Revert the Last Commit in Git

-  Update the files

         echo "Some example text for the first file." >> file-1.txt

- Add the files to the Git staging area, then commit the staged changes and push the changes:
                
        git add .
        git commit -m "Initialized repo."
        git push

- You now have a Git repository with a couple of files and several commits, which you can see listed with.

        git log


## How to Use revert on the Last Git Commit

           git log


>commit e5f9fb0d5280ab468cc0f75975d08d48308ef89c
Author: Tinku Saini <88707521+tinkusaini13@users.noreply.github.com>
Date:   Tue Jan 2 07:06:53 2024 +0000

added nginx.conf file


        git revert  e5f9fb0d5280ab468cc0f75975d08d48308ef89c

After revert command executed successfully we need to push again.

        git push

