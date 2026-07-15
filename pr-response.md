# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** 

Rename save_to_watchlist() to add_to_watchlist()

**How I verified:** 

using ctrl + f in file and ctrl+ shift + f in folder directory

## Comment 2 — Deduplication
**What I did:** 

Add deduplication logic to add_to_watchlist() in services/watchlist_service.py

**How I verified:** 

Added a test in tests/test_watchlist and ran test_add_to_watchlist_duplicate_raises, confirming a second add for the same user/film pair raises AlreadyInWatchlistError rather than silently succeeding or creating a duplicate row.

## Comment 3 — Missing test

**What I did:** 

Created test_watchlist.py and added test_add_to_watchlist_nonexistent_film_raises() test case

**How I verified:** 

Ran the test file `pytest tests/test_watchlist.py -v`

## Comment 4 — Default visibility

**My position:** 

Watchlist entries will default to `public=True`

**Reasoning:** 

Cinelog is a community film tracking app. I think that it would defeat the purpose of the community aspects of this app if all the entries in the user watchlist default to private. We would assume that, if community users are using this community platform to add to their watchlists, they would more often than not be creating public entries to be viewed by the rest of the community. Having to have them manually mark these entries as public if this did default to private would be a lot of extra work.


**Tradeoff acknowledged:** 

I do acknowledge that some users may want their watchlist entries to be private, and therefore they would need to manually change their entries to private when the default is public. There would be a period in which they have created a watchlist entry and the entry could be viewed by others. I, however, think that since there are no notifications when people add to their personal watchlists for others, it's not expected that there is going to be someone monitoring a person's watchlist 24/7. Since this is such a low-stakes sector, this privacy concern can be outweighed by the convenience and usability for the users to have their entries be defaulted to public. 

## Comment 5 — Sort order


**My position:** 

Getting watchlists should be sorted in date added order rather than alphabetical. 


**Reasoning:** 

 reasoning is that when people add something to their watchlist, it may be that they have heard a recommendation about it recently and they don't want to forget it. By having watchlists be sorted in alphabetical order, you may lose the context of someone else's entry that is being added.

As a personal anecdote, when I add books that I want to read on my read list, I kind of do it haphazardly, like when it interests me. I sometimes don't really care too much about a book that I've added two months ago versus a book that I've added a week ago. If I wanted to go and look for the name of the book that I wanted to read but I couldn't remember what it was, I would assume that the most recently added book is the book I added last week and not the one I added two months ago.


**Engagement with reviewer's point:** 

In response to the reviewers point, I agree. Most people want to see what they've added recently.

## Comment 6 — Rebase


**What conflicted:**  

When I first did the rebase, the only conflict that I saw was the.gitignore. When I fixed it, I had assumed that doing the continue command would bring me to the next conflict or the next commit, but then it didn't really seem like it. It added very cleanly, and it was strange. I did my testing, and I found out that, for some reason, it dropped the watchlist entry in the models.py without a conflict prompt at all. It was really confusing, and I had to basically hunt for the area in which it dropped because I was also worried that maybe it had overwritten my previous changes. 


**How I resolved it:** 

Since I don't have any experience with rebasing, I used Claude to help me find out what I did wrong. I was worried I did something wrong, or maybe this was just a trick. I don't know. What I did is I used `git rebase -i origin/main --edit` on the specific commit to re-add the missing watchlist entry class with UUID type fields and then matching collection entries pattern, then `commit --amend` and `rebase --continue`.


**How I verified no conflict remains:**


I verified that none of the conflicts remained by:
- running the full test
- making sure they all passed
- manually creating a film via the Flask shell
- confirming that.id returns a UUID string rather than an integer end-to-end

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->

