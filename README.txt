
Reviewed Branch
----------------

Merging to the Reviewed Branch
-------------------------------

Index::

    master - the branch that everything eventually branches from and merges to
    work - where your developers are working
    in_review - the code you currently want to review
    reviewed - a branch you merge to once the review passes

Devs should do active development in the work branch
or in a branch created from the work branch.::

    git checkout master
    git branch work

According to other team best practices, senior members of the development team should keep 'reviewed' and 'master' relatively in sync::

    git merge reviewed master
    git merge master reviewed

Merge whatever needs reviewed from the work branch to the in_review branch.::

    git checkout work
    git branch in_review
    git checkout in_review

    # You can get fancy, merging only certain commits, if you want to.
    #  This is the simplest possible example, reviewing all changes:
    git merge_work in_review

Now open the diffs in a diff visualizer.::

    # FileMerge comes with XCode on Mac: 
    >git branch
    * in_review
      master
      mock_config
      reviewed
      rhel6
    
    TODO: Vimdiff example.
     TODO: Suggest gitweb as next step for diff.

    TODO: Look around the interweb - standard process, standard branch names, etc?

When the code doesn't pass::

    # You don't need to do anything. Just leave the in_review branch.

Once the code is ready to review again::

    # Just merge the corrections into the in_revew branch again and have another review.
    git merge_work in_review

To review in GitWeb::

    >git checkout reviewed
    Switched to branch 'reviewed'
    delaport@Edwards-iMac.local:~/projects/NetIDApps
    >git push -u origin reviewed
    Total 0 (delta 0), reused 0 (delta 0)
    To https://git-sdg.cites.illinois.edu/Scratch/delaport/NetIDApps.git
     * [new branch]      reviewed -> reviewed
    Branch reviewed set up to track remote branch reviewed from origin.
    delaport@Edwards-iMac.local:~/projects/NetIDApps


Example merge command in Git::

    # Merge without 'fast forward'
    # The advantage to a non-fast-forward commit is that 
    #   it allow you to include a commit message.
    git merge in_review reviewed --no-ff -m 'Code reviewed on `date` by `whoami`'

    # You might want to create an alias


